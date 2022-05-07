Funkcja PHP Explode - dzielenie ciągu znaków według separatora
==============================================================

> id: '4dc7cec6-0e96-4a6b-aee8-32c8817ba11e'
> slug:
> 	cs: explode
> 	pl: funkcja-php-explode---dzielenie-ciagu-znakow-wedlug-separatora
> 
> perex:
> 	- Rozdělení řetězce na více částí podle oddělovače.
> 	- Podział łańcucha na wiele części zgodnie z separatorem.
> 
> publicationDate: '2019-11-26 11:39:36'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '66725772be4aae0df8af399e7ce2ba07'

Polecenie eksploduj służy do łatwego dzielenia ciągu znaków przez separator.

- Zwraca ona poszczególne wyniki w postaci tablicy numerowanej od zera,
- nie można wstawić tablicy, wprowadzany jest tylko ciąg znaków,
- nie można zmienić ogranicznika podczas przetwarzania, nie można wybrać wielu ograniczników.

| PHP 4 i nowsze |
|---------------|-----------------|
| Krótki opis | Dzielenie ciągu znaków na tablicę według separatora.
| Wymagania | Brak
| Uwaga | Nie można wstawić tablicy, tylko ciąg znaków.

Często musimy podzielić ciąg znaków według jakiejś prostej reguły. Na przykład lista liczb oddzielonych przecinkami.

Świetnie nadaje się do tego funkcja explode(), która jako pierwszy parametr przyjmuje separator (oddzielający ciąg znaków), a jako drugi - same dane:

```php
$cisla = '3, 1, 4, 1, 5, 9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
	echo $cislo . '<br>.';
	// Tutaj możemy dalej przetwarzać liczby
}
```

Ale co zrobić, jeśli liczby są oddzielone przecinkami, ale wokół nich są spacje?

Rozwiązaniem jest parsowanie według najmniejszego wspólnego podłańcucha, a następnie usunięcie niepożądanych znaków wokół niego (spacji i innych białych znaków):

```php
$cisla = '3, 1,4, 1 , 5 ,9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
     echo trim($cislo) . '<br>.';
     // Tutaj możemy dalej przetwarzać liczby
}
```

W tym przypadku funkcja `trim()` w elegancki sposób usuwa białą przestrzeń wokół znaków (spacje, tabulatory, podziały wierszy, ...), dając tylko czyste dane.

Limit, ograniczający liczbę parsowanych encji do tablicy
--------------------------

> WSKAZÓWKA: Dla wielu przykładów funkcja explode() nie jest odpowiednia i lepiej jest używać wyrażeń regularnych.

Często jednak chcemy przetwarzać dane tylko do pewnej odległości i w tym celu można użyć trzeciego (opcjonalnego) parametru limit.

Na przykład mamy dane strukturalne rozdzielone dwukropkami, w których chcemy pobrać zawartość po pierwszym dwukropku i zignorować pozostałe dwukropki.
Przykład:

```php
$cas = 'format: "j.n.Y - H:i".';
```

Gdybyśmy mieli przetworzyć ten ciąg tylko jako:

```php
$parser = explode(':', $cas);
```

Otrzymalibyśmy te 3 podłańcuchy (w innych przypadkach może być ich znacznie więcej):

```php
'format'
'"j.n.Y - H'
'i"'
```

Dlatego ustalamy limit liczby elementów, które mają zostać pobrane (i ewentualnie wszystkie pozostałe zostaną dołączone do końca ostatniego elementu):

```php
$parser = explode(':', $cas, 2);

// wróć:
echo $parser[0]; // format
echo $parser[1]; // "j.n.Y - H:i"
```

> **Uwaga:** Niechciane cudzysłowy można usunąć z łańcucha, na przykład za pomocą funkcji `trim()`:

```php
echo trim($parser[1], '"'); // drugi parametr określa mapę znaków do usunięcia
```

Pomiędzy, wstawianie łańcucha pomiędzy dwa łańcuchy
--------------------------

Często zachodzi potrzeba pobrania ciągu, który jest ograniczony przez dwa inne ciągi. Nie ma takiej funkcji bezpośrednio w PHP, ale możemy ją napisać sami:

```php
function between(string $left, string $right, string $data): string
{
   $l = explode($left, $data);
   $r = explode($right, $l[1]);

   return $r[0];
}
```

Wyrażenia regularne
--------------------------

Znacznie bardziej eleganckie dzielenie i pracę z ciągami znaków można osiągnąć za pomocą <a href="/regex">wyrażeń regularnych</a>, które omawiam na osobnej stronie.

Podobne funkcje
--------------------------

- <a href="/function-implode">Implode()</a> - konkatenacja tablicy do łańcucha znaków

Przykład parsowania adresu IP
--------------------------

```php
$ip = '10.0.0.138';
$parser = explode('.', $ip);
echo $parser[0]; // drukuje "10".
echo $parser[1]; // drukuje "0";
```

Zmienna `$ip` zawiera łańcuch wejściowy, który jest parsowany zgodnie z delimiterem `.`, a zwracana wartość jest tablicą. Przetwarzanie jest kontynuowane do końca łańcucha, chyba że zostanie określony limit.

Parametry
--------------------------

| # | Typ | Opis
|---|--------|------|
| 1 | ciąg znaków | Ciąg znaków rozdzielających.
| 2 | string | Parsowany ciąg znaków.
| 3 | int | limit parsowania. Jest to parametr opcjonalny. Przykład:

```php
$text = 'PHP to mój ulubiony język!';
$parser = explode('', $text, 1);

echo $parser[0]; // wypisuje pierwsze słowo
echo $parser[1]; // wypisuje resztę tekstu
echo $parser[2]; // nie wypisuje niczego, ponieważ został ustawiony limit!
```

Wartości zwracane
--------------------------

Wartością zwracaną jest tablica zawierająca sparsowany ciąg znaków.

Indeksy są numerowane od zera do `X`, chyba że podano ograniczenie.

Różnice w stosunku do wcześniejszych wersji
--------------------------

| Wersja PHP | Opis |
|-----------|-------|
| 5.1.0 | Dodano obsługę ujemnego limitu przepustek.
| 4.0.1 | Dodano opcjonalny parametr **limit**.

Wskazówki i uwagi
--------------------------

W przypadku użycia ujemnego **limitu** podawana jest liczba elementów od końca łańcucha.

Przykład:

```php
$str = 'jeden, dwa, trzy, cztery';

// granica dodatnia
print_r(explode('|', $str, 2));

// limit ujemny (od PHP 5.1)
print_r(explode('|', $str, -1));
```

Zostaną zwrócone następujące pola:

```php
Array
(
    [0] => one
    [1] => two|three|four
)

Array
(
    [0] => one
    [1] => two
    [2] => three
)
```
