Matematyka w PHP
================

> id: '69a432ee-7635-46e2-bb21-8492cb62c4e6'
> slug:
> 	cs: matematika
> 	pl: matematyka-w-php
> 
> perex:
> 	- 'Matematika a matematické funkce v PHP. Definice čísel, operátorů a vhodné typy pro výpočty a ukládání čísel.'
> 	- 'Matematyka i funkcje matematyczne w PHP. Definicje liczb, operatorów i odpowiednich typów do obliczeń i przechowywania liczb.'
> 
> publicationDate: '2020-02-16 18:24:10'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '1070f11c80eff1faf3388e47721b01a2'

Podobnie jak w innych językach, liczby w PHP są reprezentowane w systemie binarnym (system jedynek i zer), dlatego niektóre operacje matematyczne mogą dawać nieco inne wyniki niż te, których byśmy oczekiwali. Ta strona zawiera przegląd zachowań obliczeń w różnych kontekstach. Do zrozumienia tego zagadnienia wymagana jest przynajmniej podstawowa znajomość **technik numerycznych** i **systemów liczbowych**.

Reprezentacja liczb w pamięci
---------------------------

Sposób reprezentacji liczb jest określany przez typ danych, na przykład liczba całkowita jest konwertowana bezpośrednio z kodu źródłowego z systemu dziesiętnego na binarny. Nie musimy się w ogóle martwić o przeliczanie liczb, wszystko odbywa się automatycznie.

Konsekwencją tego przekształcenia jest to, że maksymalna wartość liczby całkowitej jest określona przez liczbę bitów dostępnych w pamięci. W 32-bitowym PHP zakres ten wynosi od `-2 147 483 648` do `-2 147 483 647` (~ ± 2 miliardy), w 64-bitowym PHP zakres ten wynosi od `-9 223 372 036 854 775 808` do `-9 223 372 036 854 775 807` (~ ± 9 kwintylionów). Wartość maksymalną można zawsze uzyskać przez wywołanie stałej `PHP_INT_MAX`.

Liczby dziesiętne są przechowywane w typie danych **float**, który ma ograniczoną precyzję, dlatego są one przechowywane wewnętrznie jako para liczb, które podlegają operacji matematycznej `mantysa * (2^wykładnik)`. Praktyczną konsekwencją tego jest możliwość przechowywania stosunkowo dużej liczby (rzędu miliardów) w małej liczbie bitów, ale niezbyt dokładnie.

Konsekwencją użycia typu danych **float** jest to, że wynik obliczenia `1 - 0,9` nie jest dokładnie `0,1`.

Przykład z <a href="https://diskuse.jakpsatweb.cz/?action=vthread&forum=9&topic=97912">forum internetowego</a>:

```php
$x = 0.3;
$y = 0.4;
echo 0.7 - $x - $y; // drukuje -5,5511151231258E-17
```

Jeśli nieco skorygujemy te liczby:

```php
$x = 6.5;
$y = 7.5;
echo 14.0 - $x - $y; // drukuje 0
```

Ogólnie rzecz biorąc, kwestia ta jest <a href="https://vtm.zive.cz/clanky/proc-pocitacum-delaji-problemy-desetinna-cisla/sc-870-a-185672/default.aspx">przedyskutowana w osobnym artykule na VTM.e15.cz: Dlaczego komputery mają problemy z liczbami dziesiętnymi</a> lub ogólnie o <a href="https://cs.wikipedia.org/wiki/Pohyblivá_řádová_čárka">punkcie zmiennoprzecinkowym w Wikipedii</a>.

> Ogólnie rzecz biorąc, dobrze jest zaokrąglić wynik po wykonaniu obliczeń lub w ogóle unikać liczb dziesiętnych. Na przykład zapisz cenę nie w koronach (np. 14,90), lecz w groszach (np. 1490), a następnie pracuj z nią dokładnie. Zaokrąglanie jest omówione w następnej części artykułu.

Operacje matematyczne
-------------------

```php
$a = 5;
$b = 3;

echo $a + $b;
```

W operacjach można stosować zwykłe konwencje matematyczne, które respektują pierwszeństwo operatorów zgodnie z zasadami matematyki (mnożenie ma pierwszeństwo przed dodawaniem itd.). Wartości można zapisywać bezpośrednio lub odczytywać za pomocą zmiennych.

> Może to nie być oczywiste na pierwszy rzut oka, ale w przykładzie matematycznym nie jest zapisany znak równości (`=`). Wynika z tego, że można rozwiązywać tylko proste operacje binarne, które zawsze działają tylko z `dwoma elementami` (nie licz równań, do tego trzeba użyć specjalistycznej biblioteki).

Przegląd podstawowych operatorów
----------------------------

> W kolumnie `wynik` wypisuję to, co jest drukowane z założeniem:

```php
$a = 5;
$b = 3;
$c = 10;
```

| Operacja | Znak | Znak słowami | Przykład pisania | Wynik
|-------------------|-----------|---------------|-----------------------|----------
| Dodawanie | `+` | Plus | `echo $a + $b;` | 8
| Odejmowanie | `-` | Minus | `echo $a - $b;` | 2
| mnożenie | `*` | gwiazdka | `echo $a * $b;` | 15
| podział | `/` | ukośnik | `echo $a / $b;` | 1.666666666666667
| nawiasy | `( )` | nawiasy | `echo $a + ($b * $c);`| 35
| Konkatenacja ciągów | `.` | Kropka | `echo $a . $b . $c;` | 5310
| Reszta po podziale | `%` | Procent | `echo $a % $b;` | 2
| dwa plusy | `echo $a++;` | 6
| odjąć jeden | `--` | dwa minus | `echo $a--;` | 4
| Moc | `**` | dwie gwiazdki | `echo $a ** $b;` | 125

> Operator potęgi (podwójna gwiazdka) jest dostępny dopiero od PHP 7.1. W innych wersjach PHP musisz używać uniwersalnej funkcji `pow($a, $b)`.

Przegląd podstawowych funkcji matematycznych
---------------------------------------

Jeśli rozwiązujesz jakieś typowe zadanie obliczeniowe, najprawdopodobniej istnieje już odpowiednia funkcja bezpośrednio w PHP - oto ich przegląd.

Zaokrąglanie
--------------

Zwykłe zaokrąglanie jest wykonywane za pomocą funkcji <a href="/function-round">round()</a>, która ma 3 parametry:

- Liczba zaokrąglona
- Opcjonalnie: Precyzja (do ilu miejsc po przecinku)
- Opcjonalnie: Wartość zmniejszona o połowę (od jakiej wartości zaokrąglić w górę)

```php
round(5);               // 5
round(5.1);             // 5
round(5.4);             // 5
round(5.5);             // 6
round(5.8);             // 6
round(5483.47621, 2);   // 5483.47
round(5483.47621, -2);  // 5500
```

Istnieje również funkcja <a href="function-floor">floor()</a> służąca do zaokrąglania w dół oraz funkcja <a href="/function-ceil">ceil()</a> służąca do zaokrąglania w górę.

> **Uwaga na typy danych!**.
>
> Wszystkie funkcje zaokrąglające zwracają wynik jako `float`, nawet jeśli wynik jest liczbą całkowitą.
>
> > Jeśli chcesz traktować wynik jako liczbę całkowitą, ważne jest, aby nadpisać typ wyniku. Na przykład: `echo (int) round(3.14);`.

PHP pracuje z różnymi typami danych, na przykład w przypadku **float** może się łatwo zdarzyć, że wynik działania funkcji `round()` nie jest liczbą o skończonym rozwinięciu dziesiętnym. Do listowania polecam użycie funkcji `number_format()`, którą omawiam poniżej.

Funkcje goniometryczne
--------------------

Są one używane do wielu różnych obliczeń i opierają się na okręgu jednostkowym. Są one używane na przykład do wykreślania okręgów, elips, przemieszczeń itp.

- <a href="/function-sin">sin()</a>.
- <a href="/function-cos">cos()</a>.
- <a href="/function-tan">tan()</a>.

Funkcje cyklometryczne
--------------------

Oblicz kąt między punktami `x` i `y`. Konwersje współrzędnych kartezjańskich i biegunowych.

- <a href="/function-asin">asin()</a>.
- <a href="/function-acos">acos()</a>.
- <a href="/function-atan">atan()</a>.

Funkcje hiperboliczne
-------------------

Na podstawie hiperboli jednostkowej. Ich definicje można łatwo opisać za pomocą porównań:

- Elipsa, Okrąg, Okrąg o promieniu 1
- Hiperbola, hiperbola równoramienna, hiperbola jednostkowa

Są one wykorzystywane na przykład do generowania terenu i symulacji fizycznych.

- <a href="/function-sinh">sinh()</a>.
- <a href="/function-cosh">cosh()</a> (chainline)
- <a href="/function-tanh">tanh()</a>.

Funkcje hiperbolometryczne
------------------------

Na podstawie hiperboli jednostkowej.

- <a href="/function-asinh">asinh()</a>.
- <a href="/function-acosh">acosh()</a>
- <a href="/function-atanh">atanh()</a>.

Przykłady zastosowania funkcji goniometrycznych
---------------------------------------

```php
echo sin(30);            // -0.98803162409286
echo sin(deg2rad(30));   // 0.5
echo cos(deg2rad(123));  // -0.54463903501503
echo 1/tan(deg2rad(45)); // 1 (onen cotangens)
echo sin(deg2rad(500));  // 0.64278760968654
echo atan2(deg2rad(50), deg2rad(23)); // 1.1396575860761
```

Kalkulator - przetwarzanie wyrażeń matematycznych
--------------------------------------------

Czasami może się zdarzyć, że będziemy musieli przetworzyć wyrażenie matematyczne jako ciąg znaków użytkownika, na przykład `5+2^(1+3/2)`.

Nie ma prostego sposobu na przetwarzanie takich danych w PHP, ale stworzyłem serię bibliotek, które zajmują się tym problemem.

Więcej szczegółów znajdziesz w osobnym artykule <a href="/pokrocila-calculator">Kalkulator w PHP: Przetwarzanie wyrażenia matematycznego jako łańcucha</a>.

Operacje na dużych liczbach i dokładność obliczeń
------------------------------------------

Duże liczby i liczby dziesiętne są przechowywane w PHP jako zmiennoprzecinkowe (float), dodatkowo są one zaokrąglane do około 15 miejsc po przecinku, co czasami może być irytujące. Dlatego właśnie stworzono bibliotekę **<a href="https://www.php.net/manual/en/book.bc.php">BCMath Arbitrary Precision Mathematics</a>**.

Liczby w tej bibliotece są reprezentowane jako **stringi**, więc nie są ograniczone niedokładnością **float**, ale wykonywane operacje są o kilka rzędów wielkości wolniejsze (ale i tak szybsze niż gdybyśmy sami zaprogramowali taką funkcję).

Na przykład, jeśli chcemy uzyskać pierwiastek kwadratowy z 2 do 3 miejsc po przecinku, wystarczy wywołać polecenie:

```php
echo bcsqrt('2', 3); // 1.414
```
