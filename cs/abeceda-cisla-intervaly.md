Získání abecedy, pole čísel a intervalů
=======================================

> id: "24f996b9-fc55-43bc-8015-093228d11577"
> slug:
> 	cs: abeceda-cisla-intervaly
> 
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1

Poměrně často potřebujeme mít k dispozici pole hodnot, které se odvozují velice jednoduchým algoritmem (třeba pole čísel od `$min` do `$max`), to lze vyřešit buď složitě:

```php
getNumbers(10, 100);

/**
 * @return int[]
 */
function getNumbers(int $min, int $max): array
{
	$numbers = [];
	for ($i = $min; $i <= $max; $i++) {
		$numbers[] = $i;
	}

	return $numbers;
}
```

Nebo použít hotovou funkci `range($min, $max, $step)`:

```php
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
foreach (range(0, 12) as $number) {
	echo $number . '; ';
}

// [0, 10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
foreach (range(0, 100, 10) as $number) {
	echo $number . '; ';
}

// ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i']
foreach (range('a', 'i') as $char) {
	echo $char . '; ';
}

// ['c', 'b', 'a']
foreach (range('c', 'a') as $char) {
	echo $char . '; ';
}
```

Tato funkce má využití třeba v modulu <a href="/paginator">Paginator</a>, který řeší stránkování dlouhého seznamu výsledků, nebo obecně zatřídění do nějakého katalogu.
