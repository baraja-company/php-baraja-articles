Obtenir l'alphabet, les tableaux de nombres et les intervalles
==============================================================

> id: '24f996b9-fc55-43bc-8015-093228d11577'
> slug:
> 	cs: abeceda-cisla-intervaly
> 	fr: obtenir-l-alphabet-les-tableaux-de-nombres-et-les-intervalles
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: cbe5f316d24908956b3bc7262f4f63b9

Très souvent nous avons besoin d'avoir un tableau de valeurs qui sont dérivées par un algorithme très simple (par exemple un tableau de nombres de `$min` à `$max`), ceci peut être résolu soit d'une manière compliquée :

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

Ou utilisez la fonction toute faite `range($min, $max, $step)` :

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

Cette fonction est utilisée par exemple dans le module <a href="/paginator">Paginator</a>, qui résout la pagination d'une longue liste de résultats, ou généralement le tri dans un certain catalogue.
