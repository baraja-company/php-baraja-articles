Echo - wyjście do kodu źródłowego
=================================

> id: d6a1a7bf-03bf-49ea-9947-d4d6753ca6e4
> slug:
> 	cs: echo
> 	pl: echo---wyjscie-do-kodu-zrodlowego
> 
> perex:
> 	- Echo - výpis řetězce do stránky a na standardní výstup. Možnosti escapování a HTTP hlaviček.
> 	- Echo - wyrzucenie łańcucha na stronę i na standardowe wyjście. Opcje dotyczące ucieczki i nagłówków HTTP.
> 
> publicationDate: '2020-02-16 18:40:31'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '7d611691825ec537f58f387696d13e28'

Konstrukcja `echo` jest używana do zrzucania zmiennej lub łańcucha do kodu źródłowego.

| Wszystkie wersje
|----------------|------
| Krótki opis: | Wypisz jeden lub więcej ciągów znaków
| Typ: <a href="/polecenia-i-funkcje">polecenie, konstrukt</a> (nie funkcja)

Opis
-----

```php
echo 'Witaj, świecie';
```

Na ekranie pojawia się napis "hello world".

```php
$var = 'Tekst';
echo $var;
```

Wypisuje wartość zmiennej `$var`, czyli "Tekst".

**Echo** nie jest funkcją (jest to <a href="/komendands-and-functions">polecenie</a>), więc możesz, ale nie musisz używać nawiasów. Zatem napisanie `echo ('hello world');` jest również poprawne.

> **Dodatkowa uwaga:** PHP traktuje **Echo** jako polecenie (konstrukt) i dlatego traktuje je jako wyrażenie. W tym przypadku nawias jest opcjonalny. Jeśli podamy notację: `echo ('coś');`, to instrukcja **Echo** nie staje się funkcją i nie jest traktowana jako taka. Nawias w tym przypadku oznacza zamknięcie dokładnej wartości wyrażenia, podobnie jak w matematyce.

Znaki cudzysłowu
--------

Ciągi znaków można ujmować w cudzysłowy i apostrofy.

A więc to:

```php
echo "Cześć";
```

To jest tak samo jak z tym:

```php
echo 'Cześć';
```

Należy jednak pamiętać, że każdy łańcuch musi zaczynać się i kończyć tym samym typem znaku cudzysłowu, a znak cudzysłowu nie może być użyty w łańcuchu.

Na przykład, jeśli chcesz wyprowadzić łącze HTML (lub dowolny kod HTML), musisz poprzedzić cudzysłów ukośnikiem. Ukośnik oznacza "dokładnie ten znak", więc nie jest rozumiany jako wyrażenie w języku.

```php
echo "<a href="index.php">tekst odnośnika</a>.";
```

Uwaga techniczna: cudzysłów ma w PHP <a href="/quotation-meaning">specjalne znaczenie</a>.

Parametry
---------

- `arg` Parametr wyjściowy.

Wartości zwracane
-----------------

Nie jest zwracana żadna wartość.

Nie może być użyty jako zmienna.

Notatka
--------

Uwaga: Ponieważ jest to **konstrukcja językowa** (construct = polecenie) (a nie funkcja), nie można jej wczytać do zmiennej.

Przykład
-------

```php
echo "Witaj, świecie";

echo "echo może wyświetlać wiele wierszy tekstu.
Należy jednak uważać na znacznik HTML <br> - nie jest on drukowany. Do tego właśnie służy funkcja nl2br()".;

$a = "php";				// definicja zmiennej

echo "Podoba mi się" . $a;	// Pisze: Lubię php
```

**Echo** ma również skróconą składnię, w której możliwe jest użycie tylko znaku równości po otwierającym znaczniku php.

```php
Ahoj <?=$jmeno;?>!
```

Jest to przydatne, gdy chcemy szybko zapisać na stronie jakąś informację. Na przykład w bieżącym roku:

```php
Píše Jan Barášek © <?=date('Y');?>
```

> Ta skrócona składnia będzie działać tylko wtedy, gdy skrócone otwierające znaczniki php są włączone, tzn. dyrektywa `short_open_tag` jest ustawiona na `on`.

Operacja
-------

Wszystkie typowe operacje matematyczne można wykonać za pomocą polecenia **echo**.

Szczegółowe omówienie matematyki można znaleźć w <a href="/matematyka">odrębnym artykule</a>.

```php
echo 5 + 3 * 2; // drukuje 11
```
