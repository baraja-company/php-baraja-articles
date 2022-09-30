Отримання алфавіту, масивів чисел та інтервалів
===============================================

> id: '24f996b9-fc55-43bc-8015-093228d11577'
> slug:
> 	cs: abeceda-cisla-intervaly
> 	uk: otrimannya-alfavitu-masiviv-cisel-ta-intervaliv
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: cbe5f316d24908956b3bc7262f4f63b9

Досить часто нам потрібно мати масив значень, які виводяться за дуже простим алгоритмом (наприклад, масив чисел від `$min` до `$max`), це може бути вирішено або складним способом:

```php
getNumbers(10, 100);

/**
 * @return int[] * @return int[]
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

Або скористатися готовою функцією `range($min, $max, $step)`:

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

Ця функція використовується, наприклад, в модулі <a href="/paginator">Paginator</a>, який вирішує питання пагінації довгого списку результатів, або взагалі сортування в деякому каталозі.
