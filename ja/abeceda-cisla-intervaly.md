アルファベット、数値の配列、間隔の取得
===================

> id: '24f996b9-fc55-43bc-8015-093228d11577'
> slug:
> 	cs: abeceda-cisla-intervaly
> 	ja: arufabetto-shu-zhino-pei-lie-jian-geno-qu-de
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: cbe5f316d24908956b3bc7262f4f63b9

非常に単純なアルゴリズムで導き出された値の配列（例えば `$min` から `$max` までの数値の配列）が必要になることはよくありますが、これは複雑な方法で解決することができます。

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

あるいは、既製の関数 `range($min, $max, $step)` を使用します。

```php
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
foreach (range(0, 12) as $number) {
	echo $number . ';';
}

// [0, 10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
foreach (range(0, 100, 10) as $number) {
	echo $number . ';';
}

// ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i' ]である。
foreach (range('a', 'i') as $char) {
	echo $char . ';';
}

// ['c', 'b', 'a'].
foreach (range('c', 'a') as $char) {
	echo $char . ';';
}
```

この関数は、たとえば<a href="/paginator">Paginator</a> モジュールで使われており、長い結果のリストのページネーションや、一般にあるカタログへのソートを解決します。
