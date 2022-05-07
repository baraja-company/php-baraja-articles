Polecenia, słowa kluczowe i funkcje w PHP
=========================================

> id: '96a00928-4410-479d-ada0-612de21cdacd'
> slug:
> 	cs: prikazy-a-funkce
> 	pl: polecenia-slowa-kluczowe-i-funkcje-w-php
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '4534cc03d2ae66454b9f398139f232a4'

Każdy skrypt PHP składa się z poleceń i funkcji, które razem nazywane są **konstrukcjami**. Podczas przetwarzania skryptu te wyrażenia językowe są śledzone (proces ten nazywany jest **tokenizacją**), a na podstawie *słów kluczowych* interpreter decyduje, jak powinien zachowywać się procesor.

Polecenia
--------------------------

Polecenia (<a href="https://www.php.net/manual/en/reserved.keywords.php">po angielsku nazywane są **keywords**</a>) są już zaprogramowanymi częściami języka, są zarezerwowane specjalnie do ich celu, można ich używać natychmiast w każdej sytuacji (nie wymagają dodatkowych specjalnych bibliotek ani instalacji) i stanowią podstawę wszystkich skryptów.

Na przykład <a href="/echo">ekstrakcja ciągu znaków do kodu HTML</a>:

```php
echo 'Echo to polecenie językowe służące do wypisywania zawartości';
```

W przypadku poleceń należy pamiętać, że nie zwracają one żadnej wartości i dlatego nie mogą być używane jako zmienne. Polecenia można również rozpoznać po tym, że nie zawierają nawiasów.

Przykłady:

```php
echo 'Witaj, świecie!';
echo ('Witaj, świecie!');
```

Jeśli treść polecenia zamknę w nawiasach, zachowanie nie ulegnie zmianie i będzie ono traktowane jako **wyrażenie**. To tak samo, jakbyśmy pisali z matematyki:

```php
5 + 3

// lub:

(5 + 3)
```

Z technicznego punktu widzenia jest to różnica, ale w praktyce nic się nie zmienia.

Funkcje
--------------------------

Gdyby istniały tylko polecenia, byłoby nudno. Funkcje są zbiorem wielu poleceń.

Często chcemy wielokrotnie wykonywać ten sam kod w wielu miejscach. W takim przypadku ciągłe kopiowanie byłoby zbyt pracochłonne i mogłoby prowadzić do błędów, dlatego zawijamy taki kod w funkcję, której nadajemy nazwę, a następnie po prostu wywołujemy ją po imieniu.

Gdy wywołujemy funkcję, przekazujemy jej parametry jako zmienne, a ona wykonuje kod i zwraca wynik, który możemy wykorzystać dalej.

```php
$text = 'PHP to mój ulubiony język!';

echo 'Tekst "' . $text . '" jest długa' . strlen($text) . 'znaki.';
```

PHP ma wiele funkcji bezpośrednio predefiniowanych (<a href="/documentation">zobacz dokumentację, aby uzyskać pełną listę</a>), ale często wygodnie jest zdefiniować własne:

```php
function mojeFunkce(int $x, int $y): int
{
   $z = $x + $y;   // dodaje parametry wejściowe i zapisuje je w zmiennej

   return $z;      // zwraca zmienną $z
}

echo mojeFunkce(5, 3); // wypisuje liczbę 8, ponieważ liczby zostały przetworzone przez funkcję
```

Zmienne w funkcjach
--------------------------

Zmienne lokalne są używane wewnątrz funkcji, co oznacza, że mogą być używane tylko wewnątrz funkcji i nie można nimi manipulować (ani ich definiować) poza funkcją. Otrzymują one swoje wartości początkowe z parametrów funkcji bezpośrednio w definicji funkcji.

Przykład:

```php
$z = 5;

function prvniFunkce(int $x, int $y): int
{
   return $x - $y; // w ten sposób zostanie zwrócona różnica w liczbach
}

function druhaFunkce(): mixed
{
   return $z; // zwraca to błąd, ponieważ zmienna $z
              // nie zdefiniowane wewnątrz funkcji
}
```

Czasami warto ustawić niektóre z parametrów jako opcjonalne - w tym celu należy zdefiniować wartość alternatywną (domyślną):

```php
echo prictiCislo(5);    // zwraca 6
echo prictiCislo(5, 7); // zwraca 12

function prictiCislo(int $x, int $y = 1): int
{
   return $x + $y;
}
```

Funkcja `prictiCislo()` dodaje wartość ze zmiennej `$y` do zmiennej `$x`. Jeśli zmienna `$y` nie jest zdefiniowana (podana jako parametr przy wywołaniu funkcji), zostanie użyta jej wartość domyślna zapisana w definicji funkcji. Drugi parametr funkcji (zmienna `$y`) jest więc opcjonalny.

> Każda funkcja może mieć tylko jedno wyjście (jeden zwrot), jeśli w funkcji podano wiele wyjść, zwrócone zostanie to, które podano jako pierwsze. Jeśli chcesz zwrócić wiele wartości, musisz użyć tablicy.
>
> W PHP (w odróżnieniu od innych języków) nie musimy w ogóle określać zwrotu - w takim przypadku funkcja nie zwraca nic (void) na dowolnych danych wejściowych. Takie funkcje są najczęściej używane do przechowywania danych lub wyprowadzania danych do kodu źródłowego. Ogólnie jednak zaleca się, aby zawsze zwracać jakąś wartość, a przynajmniej potwierdzenie, że wszystko poszło dobrze.
