Zasady pisania zmiennych
========================

> id: '4c8e631b-b305-4169-8885-4f9506155999'
> slug:
> 	cs: zasady-promennych
> 	pl: zasady-pisania-zmiennych
> 
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '5528d9b73c1d1c07c330a58e4aeaa06a'

Jest to druga część serii poradników na temat języka PHP. W tym odcinku zapoznamy się z podstawowymi zasadami pisania zmiennych.

Ta strona to tylko krótki przegląd. Jeśli szukasz szczegółowego opisu technicznego wszystkich funkcji, napisałem <a href="/zmienna">oddzielny artykuł</a>.

Składnia podstawowa
--------------------------

Zmienne w PHP zaczynają się od znaku dolara `$`, po którym następuje nazwa.

```php
$zvire = 'cat';
```

Ciągi (sekwencje znaków) są ujęte w cudzysłowy lub apostrofy:

```php
$a = "Znaki cudzysłowu";
$b = 'apostrofy';
```

Cyfry nie są ujmowane w cudzysłów:

```php
$a = 5;
$b = 10;
$c = 3.14159;
```

Nazwa zmiennej może składać się tylko ze znaków alfabetu angielskiego i cyfr. Nazwa zawsze zaczyna się od litery.

Jeśli nazwa składa się z więcej niż jednego słowa, zwyczajowo stosuje się składnię `camelCase` (pierwsza litera mała, a każde inne słowo zaczyna się wielką literą):

```php
$kocka = 'Kitty';
$rychlyPocitac = 'Oczywiście, że to moje!';
$pocetRohuJednorozce = 1;
```

Nazwa nie może zawierać spacji, myślników, haczyków, przecinków, cudzysłowów, nawiasów ani innych znaków specjalnych. Jedynym dozwolonym znakiem specjalnym jest `podkreślenie`.

Liczby dziesiętne zapisuje się z kropką:

```php
$pi = 3.14159;
```

Często przydatne może być wykonywanie operacji matematycznych bezpośrednio podczas definiowania zmiennej:

```php
$a = 5;
$b = 3;
$c = $a + $b;	// dodaj 5 + 3
echo $c;		// drukuje 8
```

Poprawne wstawienie cudzysłowu lub apostrofu
--------------------------

Nie wolno dowolnie łączyć cudzysłowów i apostrofów. Na przykład, jeśli zdecydujemy się na użycie cudzysłowu, musimy również zakończyć łańcuch cudzysłowem, a nie używać go wewnątrz.

Dlatego jest to błędne:

```php
echo "<img src="obrazek.gif">";
```

Ponieważ nie jest jasne, gdzie zaczyna się i kończy łańcuch. Dlatego cudzysłowy i apostrofy nie mogą być zagnieżdżane.

Jednym z możliwych rozwiązań jest <a href="/escapovani">escaping</a>, w którym problematyczny znak jest poprzedzony odwrotnym ukośnikiem.

```php
echo "<img src="image.gif">.";
```

Odwrotny ukośnik oznacza, że następnym znakiem będzie dokładnie ten, którego chcemy użyć.

Jednak w przypadku kodu HTML lepiej jest objąć cały łańcuch apostrofami, a następnie użyć cudzysłowów w normalny sposób:

```php
echo '<img src="image.gif">.';
```

Można też odwrócić ten proces:

```php
echo "<img src='obrazek.gif'>.";
```

Wypełnianie zmiennej z adresu url lub z formularza
--------------------------

Adresy zawierające znak zapytania przenoszą informacje o zmiennych wejściowych, tak więc na przykład `index.php?page=kontakty` oznacza zmienną `strona` z wartością `kontakty`. Wartość tej zmiennej jest odczytywana jako `$_GET['strona']`.

Znak zapytania nie jest w żaden sposób związany z nazwą pliku na dysku. Jest to zawsze ten sam plik, do którego przekazujemy parametry w adresie.

Szczegółowo omawiam tę kwestię w artykule na temat <a href="/metody-odesilani-dat">metod przesyłania danych</a>.

Określanie zawartości zmiennej na podstawie adresu
--------------------------

Niektóre zmienne są dostępne już w momencie uruchamiania skryptu (i dlatego można ich od razu używać) - są to **zmienne superglobalne**. Na przykład, jeśli chcemy odczytać wartość z adresu URL, używamy zmiennej `$_GET`.
Sposób użycia jest następujący:

```php
$a = $_GET['a'];

echo $a;
```

Ten skrypt wypisuje do kodu źródłowego to, co znajduje się w adresie URL po znaku zapytania.

> Ostrzeżenie, ta próbka nie jest bezpieczna! Jeśli nieuczciwy użytkownik wprowadziłby w adresie URL na przykład kod HTML, zostałby on umieszczony na stronie i wykonany. Dlatego zawsze musimy traktować dane wyjściowe; służy do tego funkcja `htmlspecialchars()`.

```php
$a = $_GET['a'];

echo htmlspecialchars($a);
```

> Jeśli wejdziemy na stronę bez podania parametru `?a=anything`, zmienna `$_GET['a']` nie będzie istnieć i PHP wyrzuci komunikat o błędzie. Musimy potraktować ten warunek jako warunek i nie robić nic, jeśli zmienna nie istnieje (lub ewentualnie wypisać alternatywną treść). Istnienie zmiennej można sprawdzić za pomocą funkcji `isset()`.

```php
if (isset($_GET['a'])) {
    $a = $_GET['a'];

    echo htmlspecialchars($a);
} else {
    echo 'Zmienna "a" nie istnieje!';
}
```

Przykład z liczeniem
--------------------------

Mając zmienne z adresu URL, możemy wykonać zadania dla psów, na przykład zsumować je i bezpośrednio wypisać wynik:

```php
echo $_GET['a'] + $_GET['b'];
```

Jeśli chcemy umieścić w adresie URL więcej parametrów wejściowych, musimy oddzielić je znakiem ampersand (`&`). Adres ten może wyglądać następująco: `index.php?a=5&b=3`.

Łączenie wejść tekstowych (ciągów znaków)
--------------------------

Można także łatwo połączyć 2 wejścia tekstowe (ciągi znaków). Służy do tego operator kropki. Łącze można zamieścić w zmiennej lub podczas wpisywania na listę.

```php
$a = 'pies';
$b = 'cat';

echo $a . 'a' . $b;
```

Jest tam napisane "pies i kot".
