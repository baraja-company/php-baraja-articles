Zmienne w PHP
=============

> id: b89774e7-143c-4a8c-8dc6-3b3d2c78d5b7
> slug:
> 	cs: promenna
> 	pl: zmienne-w-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '23e4d537f18a3b2e1946c79be1aacf52'

Ta strona jest kompletnym podsumowaniem tego, jak działają zmienne w PHP. Tekst jest napisany w nieco technicznym stylu i może nie być w pełni zrozumiały dla początkujących. Jeśli interesują Cię kompletne podstawy, przeczytaj <a href="/first-script">samouczek dla początkujących</a> oraz <a href="/principles-of-prominent-script">zasady pisania zmiennych</a>.

Opis
-----

Zmienna to wirtualne miejsce w pamięci operacyjnej, zdefiniowane przez `nazwa` i `typ danych`. W ramach typu danych zmienna ma wtedy pewną `treść`.

PHP wewnętrznie reprezentuje zmienne jako tzw. tablicę haszującą, tzn. wszystkie zmienne są przechowywane w tablicy, która jest bardzo szybka do przeszukiwania, więc czas potrzebny na dostęp do każdej zmiennej jest *prawie* stały.

Przykłady pisania:

```php
$a = 10;
$b = 'cat';
$c = true;
```

Każdy wiersz w próbie oznacza definicję jednej zmiennej. Nazwa każdej zmiennej zaczyna się od znaku dolara `$`, po którym następuje sama nazwa. Znak równości może być użyty do przypisania wartości zmiennej.

Wewnętrznie zmienne będą przechowywane w pamięci w postaci tablicy hash:

| Nazwa | Typ | Skrót | Wartość |
|-------|---------|---------|---------|
| `$a` | integer | int | `10` |
| `$b` | string | string | `cats` |
| `$c` | boolean | bool | `true` |

Typy zmiennych
---------------

Zmienne są klasyfikowane zgodnie z prawami dostępu i sposobem wykorzystania:

- <a href="/local-variable">zmienne lokalne</a> (ważne w kontekście, tzn. w ramach funkcji i metod),
- <a href="/global-variable">zmienne globalne</a> (obowiązujące w całym skrypcie),
- <a href="/superglobal-variable">zmienne superglobalne</a> (obowiązujące w całym środowisku serwera - zazwyczaj dane użytkownika),
- <a href="/promenna-zmienna">zmienna zmienna</a> (specjalna zmienna, która zawiera odniesienie (link) do innej zmiennej).

* Nie należy używać `zmiennych globalnych` i `zmiennych zmiennych`, ponieważ przyczyniają się one do nieczytelności kodu i "magicznego" (nieoczekiwanego) zachowania aplikacji.

Dozwolona zawartość zmiennych
--------------------------

Zmienne mogą zawierać wszystko, na co pozwala ich aktualny typ danych. Jeśli typ danych nie zostanie określony, zostanie on ustalony automatycznie na podstawie zawartości (nie jest to zalecane, ponieważ może prowadzić do błędów).

Typy danych działają niezależnie, więc możemy używać prawie każdego typu. Jeśli jednak chcemy wykonać jakąś operację scalania, musimy zawsze zapewnić konwersję do jednego formatu.

Dobrym przykładem jest np. dodawanie i mnożenie liczb:

```php
$x = 5;       // integer
$y = 3;       // integer
$z = $y + $y; // zmienna $z zostanie utworzona na podstawie wielu zmiennych
```

W tym przypadku PHP staje przed pytaniem, jaki typ danych będzie miała nowo utworzona zmienna `$z`. Jeśli są one tego samego typu danych i operacja jest możliwa, typ danych jest dziedziczony.

Czasami jednak można wykonać operację na wielu typach danych:

```php
$x = 1;       // integer
$y = 3.14159; // float
$z = $y + $y; // float
```

W tym przypadku łączymy liczby integer i float. Dane wyjściowe będą liczbami dziesiętnymi, dlatego używana jest funkcja float. W takim przypadku PHP wykona coś, co nazywa się **dynamicznym repartycjonowaniem**.

Nie zawsze jednak możemy polegać na tym zachowaniu. Na przykład, jak chcesz połączyć liczbę i ciąg znaków?

```php
$x = 256;     // integer
$y = 'Hej! Hej!'; // float
$z = $y + $y; // ???
```

Typy danych (przegląd najważniejszych z nich)
--------------------------------------

PHP jest językiem interpretowanym, ma więc pewne osobliwości w porównaniu z innymi językami programowania. Jedną z nich jest to, że nie musimy określać typów danych dla zmiennych, tzn. każda zmienna dynamicznie zmienia swój typ danych w zależności od swojej zawartości (chyba że zaznaczono inaczej). Dobrze jest jednak znać przynajmniej podstawowe typy danych, co jest szczególnie przydatne przy optymalizacji aplikacji i pracy z bazami danych.

Notacja może wyglądać następująco:

```php
$x = (int) 25; // tworzy zmienną typu integer
```

<a href="/datove-typy">Przegląd typów danych</a>.

Dziedziczenie typów danych
-----------------------

Jaki typ danych będzie miała zmienna `$x`, jeśli znamy tylko ten fragment kodu źródłowego?

```php
$x = $y;
```

Zależy to od typu danych zmiennej `$y`, po której dziedziczona jest zarówno wartość, jak i jej typ danych. W tym przypadku nie znamy zmiennej `$x`, więc nie możemy kontynuować obliczania kodu i zostałby wyświetlony komunikat o błędzie.

Zastąpienie dynamiczne
---------------------

Mamy następujące dwie zmienne:

```php
$x = 10;
$y = '10';
```

Jaka jest różnica między zawartością zmiennych `$x` i `$y`?

Zmienna `$x` jest liczbą, `$y` jest łańcuchem (zawierającym "1" i "0"), więc jeśli tylko przechowamy zmienną w pamięci i nie wykonamy żadnej operacji, która wpłynie na jej wartość. Na przykład dwa poniższe wpisy dadzą ten sam wynik:

```php
echo $x + 5;	// drukuje 15
echo $y + 5;	// drukuje 15
```

W drugim przypadku następuje tak zwane **nadpisanie dynamiczne**, czyli zmienna zmienia swój typ danych tak, aby można było wykonać na niej operację obliczeniową. Nie zawsze można polegać na tym zachowaniu i jest ono raczej zachowaniem korekcyjnym, służącym do poprawiania źle napisanych skryptów dla początkujących. Jeśli to możliwe, zawsze zapisuj liczby z typem danych do ich przechowywania, ponieważ zwiększa to ich precyzję i ułatwia ich wykorzystanie w przyszłości.

> **Uwaga:** Należy pamiętać, że nie możemy konwertować typów danych w sposób całkowicie dowolny, więc nie zawsze jest to możliwe. W przypadku nadpisania typu danych na inny (niekompatybilny) typ, konwersja może w ogóle nie nastąpić, albo oryginalna zawartość może zostać uszkodzona lub całkowicie zniszczona i zastąpiona inną. Na przykład, jeśli łańcuch znaków zostanie przekształcony na liczbę całkowitą (a w zmiennej zostanie zapisany tekst, który nie jest liczbą), w zmiennej zostanie zapisana wartość `1` zamiast wartości liczbowej.

Reprezentowanie ciągów znaków jako tablic
------------------------------

Wszystkie łańcuchy są przechowywane wewnętrznie jako tablica znaków. Oznacza to, że każdy znak ma swój własny **indeks** i można się do niego odwoływać. Jeśli nie określimy indeksu, przetwarzany jest cały łańcuch.

```php
$x = 'Zaprogramujmy się w PHP!';
$n = 3;

echo $x;		// w ten sposób zostanie wypisana cała zawartość zmiennej $x
echo $x[0];		// w ten sposób zostanie wypisany znak zerowy zmiennej $x
echo $x[$n];	// w ten sposób zostanie wypisany n-ty znak zmiennej $x
```

> **Uwaga:** Numery PHP od zera, tzn. znak zerowy to "P", a pierwszy znak to "r".
>
> > Dodatkowo znaki są przełączane bajtami. Na przykład znak "no" w kodowaniu UTF-8 ma długość 2 bajtów, więc indeks znaku w łańcuchu nie będzie odpowiadał rzeczywistej pozycji podczas przewijania, a do zapisania znaku zostaną użyte 2 indeksy.

Istnienie elementu tablicy powinno być zawsze sprawdzane za pomocą funkcji `isset()`:

```php
if (isset($x[$n])) {
    echo $x[$n];
}
```

Można też zapisać to ładnie za pomocą operatora trójskładnikowego:

```php
echo $x[$n] ?? '';
```

Kopiowanie zmiennych
---------------------

Mamy następującą zmienną:

```php
$q = 'Lorem ipsum, ...';
```

A następnie skopiuj jej wartość do następnej zmiennej:

```php
$qi = $q;
```

Na szczęście nie zostanie wykonane żadne kopiowanie, a PHP po prostu przechowa referencję do wartości w **tablicyhash**. Wartość zostanie faktycznie skopiowana tylko wtedy, gdy wartość jednej ze zmiennych ulegnie zmianie. Zachowaniem tym zajmuje się komponent zwany ogólnie **kolektorem Garbridge**.
