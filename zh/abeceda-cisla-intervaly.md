获取字母表、数字数组和区间表
==============

> id: '24f996b9-fc55-43bc-8015-093228d11577'
> slug:
> 	cs: abeceda-cisla-intervaly
> 	zh: huo-qu-zi-mu-biao-shu-zi-shu-zu-he-qu-jian-biao
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: cbe5f316d24908956b3bc7262f4f63b9

很多时候，我们需要有一个由非常简单的算法得出的数值数组（例如一个从`$min`到`$max`的数字数组），这既可以用复杂的方法解决。

```php
getNumbers(10, 100);

/**
 *返回int[]
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

或者使用现成的函数`range($min, $max, $step)`。

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

例如，这个函数用于<a href="/paginator">Paginator</a>模块，它解决了一长串结果的分页问题，或者一般情况下分拣到一些目录。
