Obtención del alfabeto, matrices de números e intervalos
========================================================

> id: '24f996b9-fc55-43bc-8015-093228d11577'
> slug:
> 	cs: abeceda-cisla-intervaly
> 	es: obtencion-del-alfabeto-matrices-de-numeros-e-intervalos
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: cbe5f316d24908956b3bc7262f4f63b9

Muy a menudo necesitamos tener un array de valores que se derivan por un algoritmo muy simple (por ejemplo un array de números de `$min` a `$max`), esto se puede resolver bien de una manera complicada:

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

O utilice la función ya preparada `range($min, $max, $step)`:

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

Esta función se utiliza, por ejemplo, en el módulo <a href="/paginator">Paginador</a>, que resuelve la paginación de una lista larga de resultados, o en general la ordenación en algún catálogo.
