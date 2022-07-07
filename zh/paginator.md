PHP中的分页器和结果的分页
==============

> id: a1450160-e320-414a-8266-128465632e94
> slug:
> 	cs: paginator
> 	zh: php-zhong-de-fen-ye-qi-he-jie-guo-de-fen-ye
> 
> perex:
> 	- Stránkování dlouhého seznamu položek. Jak řešit omezení počtu vypsaných položek a vypočítat stránkování.
> 	- 对一长串的项目进行分页。如何解决上市项目数量的限制和计算分页。
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: d0bc71ded032401875396b0fd263a820

当我们有大量的数据需要倾倒时，将其分成多个页面是很有礼貌的。本文不涉及传递页码和列出结果的实际执行，只涉及理论上的提取数值和计算最佳编码本，以使浏览大量页面尽可能方便用户。

我们有多少个结果
----------------------

首先，我们需要找出我们到底有多少个结果。如果数据来自数据库，可以用以下SQL语句非常有效地进行统计。

```sql
SELECT COUNT(*) FROM tabulka
```

计算速度非常快，因为数据库将统计数据保存在一个辅助文件中，所以它根本不接触数据。

如果数据来自其他地方（例如我们把它放在一个数组中），可以用count()函数进行计数。

```php
$cisla = [3, 1, 4, 1, 5, 9, 2];

echo '该领域包含' . count($cisla) . '数字。';
```

限制结果的数量
----------------------

另一个问题是对结果数量的限制。如果数据在数据库中，只需在SQL语句中放入`LIMIT`参数。

```sql
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10
```

这个命令总是能得到最多10个结果，而且它还能使查询速度加快，因为数据库不必翻阅整个数据文件。

如果我们有其他来源的数据（同样是一个数组），我们也可以使用`$iterator`帮助变量在PHP级别上限制结果。

```php
$pole = [...];

$iterator = 0;
$limit = 10;
foreach ($pole as $prvek) {
	// 这里是数据被转储的地方

	$iterator++;
	if ($iterator >= $limit) {
	    break; // 当循环运行了10次后停止。
	}
}
```

跳过最初的X个结果
----------------------

当我们在第一页时，非常简单，你只需要用`LIMIT`限制结果的数量。但如果我在第三页呢？那么我们就必须跳过第一个`X'的结果。

在SQL中，我们又有一个优雅的符号来表示。

```sql
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10 OFFSET 20
```

它跳过前20个结果，将下一个输出限制在10个结果，所以它输出的区间是`<21 - 30>`。

在纯 PHP 中，有两种处理方式。

如果我们知道数组的索引，我们就可以从某一点开始读它（这非常快）。

```php
$pole = [...];

$start = 20;
$limit = 10;
for ($i = $start; ($i <= $start + $limit && isset($pole[$i])); $i++) {
	// 这里是数据被转储的地方
}
```

然而，对于一个未知的字段，我们必须再次使用迭代器并跳过这些项目。

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

	// 以某种方式将数据转储到这里

	$iterator++;
	if ($iterator >= $start + $limit) break;
}
```

显示最佳分页器/步进器
----------------------

假设我们知道项目的总数，页面上的项目数，以及当前的页码。现在我们要渲染一个酒吧，允许快速浏览所有带有搜索结果的页面。然而，由于有许多网页（大约有数千页），我们不可能一下子把它们都列出来，所以我们必须明智地选择一些有代表性的，最能代表网页之间的范围的。

它可能看起来像这样。

```php
1 | 15 | 30 | 36 | 45 | 60 | 72
```

任务。

我在72页中的第36页，如何最佳地放置页码？
嗯，通过顺序。

> **提示：**通过实际观察，我发现Paginator的左边部分应该通过算术序列来计算（所以我可以通过相同的步数线性移动），而右边部分则通过**几何序列来计算，这又使我很容易迈出大步。因此，如果我想进入一个特定的页面，我首先会跳过大量不必要的项目，然后通过回到左边来细化选择。

算术序列理论（我们不断增加同一个数字）。

```php
$d = 10;   // 步距大小
$a[1] = 1; // 第一个元素
$a[2] = $a[1] + $d; // 第二个元素
$a[3] = $a[1] + 2 * $d;
$a[3] = $a[2] + $d;
$a[$n] = $a[1] + ($n - 1) * $d; // 第n个元素

function getAritmeticItem(int $start, int $step, int $n): int
{
	return $start + ($n - 1) * $step;
}
```

几何序列理论（总是乘以同一个数字）。

```php
$q = 10;   // 步距大小
$a[1] = 1; // 第一个元素
$a[2] = $a[1] * $q; // 第二个元素
$a[3] = $a[1] * $q * $q;
$a[3] = $a[1] * pow($q, 2);
$a[3] = $a[2] * $q;
$a[$n] = $a[1] * pow($q, $n - 1); // 第n个元素

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
