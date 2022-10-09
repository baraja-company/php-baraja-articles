Hentning af alfabetet, talrækker og intervaller
===============================================

> id: '24f996b9-fc55-43bc-8015-093228d11577'
> slug:
> 	cs: abeceda-cisla-intervaly
> 	da: hentning-af-alfabetet-talraekker-og-intervaller
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: cbe5f316d24908956b3bc7262f4f63b9

Ofte har vi brug for et array af værdier, der er afledt af en meget simpel algoritme (f.eks. et array af tal fra `$min` til `$max`), dette kan løses enten på en kompliceret måde:

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

Du kan også bruge den færdige funktion `range($min, $max, $step)`:

```php
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
foreach (range(0, 12) as $number) {
	echo $number . ';';
}

// [0, 10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
foreach (range(0, 100, 10) as $number) {
	echo $number . ';';
}

// ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i']
foreach (range('a', 'i') as $char) {
	echo $char . ';';
}

// ['c', 'b', 'a']
foreach (range('c', 'a') as $char) {
	echo $char . ';';
}
```

Denne funktion bruges f.eks. i modulet <a href="/paginator">Paginator</a>, som løser paginering af en lang liste af resultater eller generelt sortering i et katalog.
