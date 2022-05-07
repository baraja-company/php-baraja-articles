Paginator i stronicowanie wyników w PHP
=======================================

> id: a1450160-e320-414a-8266-128465632e94
> slug:
> 	cs: paginator
> 	pl: paginator-i-stronicowanie-wynikow-w-php
> 
> perex:
> 	- Stránkování dlouhého seznamu položek. Jak řešit omezení počtu vypsaných položek a vypočítat stránkování.
> 	- Paginacja długiej listy elementów. Jak rozwiązać problem ograniczenia liczby wyświetlanych elementów i obliczać paginację.
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: d0bc71ded032401875396b0fd263a820

Gdy mamy dużo danych do zrzucenia, dobrze jest podzielić je na wiele stron. Niniejszy artykuł nie zajmuje się praktyczną realizacją przekazywania numerów stron i listowania wyników, a jedynie teoretycznym wyodrębnianiem wartości i obliczaniem optymalnej książki kodowej, aby przeglądanie dużej liczby stron było jak najbardziej przyjazne dla użytkownika.

Ile mamy wyników
----------------------

Na początek należy sprawdzić, ile wyników mamy w ogóle. Jeśli dane pochodzą z bazy danych, można je bardzo sprawnie policzyć za pomocą następującego polecenia SQL:

```sql
SELECT COUNT(*) FROM tabulka
```

Obliczenia są bardzo szybkie, ponieważ baza danych przechowuje statystyki w pliku pomocniczym, a więc w ogóle nie dotyka danych.

Jeśli dane pochodzą z innego miejsca (i mamy je np. w tablicy), można je policzyć za pomocą funkcji count():

```php
$cisla = [3, 1, 4, 1, 5, 9, 2];

echo 'Pole zawiera' . count($cisla) . 'numery.';
```

Ograniczanie liczby wyników
----------------------

Innym problemem jest ograniczenie liczby wyników. Jeśli dane znajdują się w bazie danych, wystarczy umieścić parametr `LIMIT` w instrukcji SQL:

```sql
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10
```

Dzięki temu poleceniu zawsze otrzymamy maksymalnie 10 wyników, a zapytanie będzie przebiegać szybciej, ponieważ baza danych nie będzie musiała przeglądać wszystkich plików danych.

Jeśli mamy dane z innego źródła (znowu tablica), możemy również ograniczyć wyniki na poziomie PHP, używając zmiennej pomocniczej `$iterator`:

```php
$pole = [...];

$iterator = 0;
$limit = 10;
foreach ($pole as $prvek) {
	// w tym miejscu dane są zrzucane

	$iterator++;
	if ($iterator >= $limit) {
	    break; // Zatrzymuje cykl po 10-krotnym uruchomieniu.
	}
}
```

Pomijanie pierwszych X wyników
----------------------

Gdy jesteśmy na pierwszej stronie, sprawa jest dość prosta, wystarczy ograniczyć liczbę wyników za pomocą `LIMIT`. Ale co, jeśli jestem na trzeciej stronie? Następnie należy pominąć pierwsze `X` wyników.

W języku SQL mamy na to elegancką notację:

```sql
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10 OFFSET 20
```

Pomija pierwsze 20 wyników i ogranicza kolejne wyjście do 10 wyników, tak że wypisuje przedział `<21 - 30>`.

W czystym PHP jest to obsługiwane na dwa sposoby.

Jeśli znamy indeksy tablicy, możemy rozpocząć jej odczytywanie od określonego punktu (co jest bardzo szybkie):

```php
$pole = [...];

$start = 20;
$limit = 10;
for ($i = $start; ($i <= $start + $limit && isset($pole[$i])); $i++) {
	// w tym miejscu dane są zrzucane
}
```

Jednak w przypadku nieznanego pola musimy ponownie użyć iteratora i pominąć elementy:

```php
$pole = [...];

$iterator = 0;
$start = 20;
$limit = 10;
foreach ($pole as $prvek) {
	if ($iterator < $start) {
		$iterator++;
		continue;
	}

	// w jakiś sposób dane są zrzucane tutaj

	$iterator++;
	if ($iterator >= $start + $limit) break;
}
```

Wyświetlanie optymalnego paginatora/steppera
----------------------

Załóżmy, że znamy całkowitą liczbę elementów, liczbę elementów na stronie oraz bieżący numer strony. Teraz chcemy wyrenderować pasek, który pozwoli na szybkie przeglądanie wszystkich stron z wynikami wyszukiwania. Ponieważ jednak stron jest bardzo dużo (rzędu tysięcy), nie możemy wymienić ich wszystkich naraz, dlatego musimy inteligentnie wybrać kilka reprezentatywnych, które najlepiej odzwierciedlają rozpiętość między stronami.

Może to wyglądać następująco:

```php
1 | 15 | 30 | 36 | 45 | 60 | 72
```

Przydział:

Jestem na stronie 36 z 72, jak optymalnie umieścić numery stron?
Cóż, poprzez sekwencję.

> **Wskazówka:** Dzięki praktycznej obserwacji odkryłem, że lewą część Paginatora należy liczyć ciągiem arytmetycznym (dzięki czemu można się poruszać liniowo o tę samą liczbę kroków), a prawą - ciągiem **geometrycznym**, co z kolei ułatwia wykonanie dużego kroku. Jeśli więc chcę przejść do konkretnej strony, najpierw pomijam dużą liczbę niepotrzebnych elementów, a następnie uściślam wybór, cofając się do lewej strony.

Teoria ciągów arytmetycznych (dodajemy wciąż tę samą liczbę):

```php
$d = 10;   // wielkość kroku
$a[1] = 1; // pierwszy element
$a[2] = $a[1] + $d; // drugi element
$a[3] = $a[1] + 2 * $d;
$a[3] = $a[2] + $d;
$a[$n] = $a[1] + ($n - 1) * $d; // n-ty element

function getAritmeticItem(int $start, int $step, int $n): int
{
	return $start + ($n - 1) * $step;
}
```

Teoria ciągów geometrycznych (mnożenie zawsze przez tę samą liczbę):

```php
$q = 10;   // wielkość kroku
$a[1] = 1; // pierwszy element
$a[2] = $a[1] * $q; // drugi element
$a[3] = $a[1] * $q * $q;
$a[3] = $a[1] * pow($q, 2);
$a[3] = $a[2] * $q;
$a[$n] = $a[1] * pow($q, $n - 1); // n-ty element

function getGeometricItem(int $start, int $step, int $q): int
{
	return $start * pow($q, $step - 1);
}
```



```php
$start = 1;
$current = 36;
$end = 72;
```
