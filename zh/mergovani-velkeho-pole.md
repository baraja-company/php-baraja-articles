在PHP中合并大型数组
===========

> id: ccfb3f47-c8a0-4d0f-aa29-18d4c1775725
> slug:
> 	cs: mergovani-velkeho-pole
> 	zh: zaiphp-zhong-he-bing-da-xing-shu-zu
> 
> publicationDate: '2020-02-06 09:32:08'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3bfc3b53d26a4c9b675e851a643fc71a'

通常我们需要将多个数组合并在一起，这可以通过`array_merge`函数非常优雅地完成。

```php
$userIdsA = [1, 2, 3];
$userIdsB = [5, 6, 7];

// 返回 [1, 2, 3, 5, 6, 7] 。
$finalIds = array_merge($userIdsA, $userIdsB);
```

`array_merge`函数将两个数组合并为一个大数组。如果出现键的碰撞，则最右边的数组的值获胜。

循环中的重复合并
---------------------------

然而，我们经常得到一个只在循环中创建的数组（例如，从数据库中获得，然后通过foreach传递），因此我们并不预先知道合并的数量。

一个天真的解决方案可能看起来像这样。

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds = array_merge($finalIds, $user->someIds);
}
```

然而，这种解决方案的CPU效率很低，因为我们必须在每次迭代时将数组合并，并在整个大数组上进行迭代。

然而，有一个简单的解决方案，即我们修改合并算法，使其只经过一次数据。

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds[] = $user->someIds;
}
```

在这种情况下，`$finalIds'字段将产生更多的数据，但与节省时间的优势相比，这仍是一个较小的问题。

合并本身取决于你所使用的PHP版本，并通过一个优雅的技巧来解决。

```php
/* PHP 5.6及以上版本 */
$finalIds = call_user_func_array('array_merge', $finalIds + [[]]);

/* PHP 5.6以上版本 */
$finalIds = array_merge([], ...$finalIds);

/* PHP 7.4以上版本的非空字段 */
$finalIds = array_merge(...$finalIds);
```

特别是`array_merge(...$finalIds)`的解决方案看起来非常有趣，因为它使用了PHP7的一个新概念，可以使用开头的三连字符向一个函数传递动态的参数数。然后，合并过程尽可能地高效，整个逻辑由PHP内部自动处理。

速记符号`array_merge(...$finalIds)`只能用于非空的数组。如果是空数组，则没有参数传递给函数，函数会抛出一个 "Function array_merge invoked with 0 parameters, at least 1 required. "错误。
