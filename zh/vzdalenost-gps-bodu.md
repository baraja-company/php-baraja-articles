两个GPS点之间的距离
===========

> id: '9e67de8f-922a-4f96-886a-07f0002b66ad'
> slug:
> 	cs: vzdalenost-gps-bodu
> 	zh: liang-gegps-dian-zhi-jian-de-ju-li
> 
> publicationDate: '2022-12-18 11:00:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: d466d432cddc147949c75ef28fbaf503

两个GPS点的近似距离可以通过该算法轻松计算出来。

PHP的实施
------------------

```php
function getCoordsDistance(
	float $lat1,
	float $lng1,
	float $lat2,
	float $lng2
): float {
	$r = 6371;
	$lat1 = ($lat1 / 180) * M_PI;
	$lat2 = ($lat2 / 180) * M_PI;
	$lng1 = ($lng1 / 180) * M_PI;
	$lng2 = ($lng2 / 180) * M_PI;

	$x = ($lng2 - $lng1) * cos(($lat1 + $lat2) / 2);
	$y = $lat2 - $lat1;
	$d = sqrt($x * $x + $y * $y) * $r;

	return round($d * 1000);
}
```

这个函数假设地球是一个完美的球体，所以输出结果只能作为一个估计。对于一个现实的距离计算，你仍然需要考虑到海拔高度、地形形状和局部平坦化。

我使用这个功能来估计电子商店网点的距离。在捷克共和国，每1公里的平均偏差约为5米，这在实际操作中是足够的。

在TypeScript中实现
--------------------------

你可能需要在客户端进行计算，这就是javascript的用武之地。

```js
export const getCoordsDistance = (
    lat1: number,
    lng1: number,
    lat2: number,
    lng2: number
): number => {
  const r = 6371;
  const subLat1 = (lat1 / 180) * Math.PI;
  const subLat2 = (lat2 / 180) * Math.PI;
  const subLng1 = (lng1 / 180) * Math.PI;
  const subLng2 = (lng2 / 180) * Math.PI;

  const x = (subLng2 - subLng1) * Math.cos((subLat1 + subLat2) / 2);
  const y = subLat2 - subLat1;
  const d = Math.sqrt(x * x + y * y) * r;

  return Math.round(d * 1000 * 100000000) / 100000000;
};
```

如何在数据库中搜索附近的地方
------------------------------------

要在数据库中找到特定点周围的位置，你可以在SQL中直接使用上面列出的函数的昂贵计算，或者你可以聪明地做到这一点。

就个人而言，我使用一对算法的组合来寻找附近的位置（例如，从电子商店到客户结账的分支机构）。

在第一步中，你只是在选定的点周围加载一个正方形区域（你通过简单地从所需的点中减去一个常数值，然后搜索这个区间来获得）。这给了我们一个候选结果的列表，但我们还不知道它们到底有多大的差距，也不知道它们的顺序。

在第二步中，我们需要弄清楚我们是在寻找特定圆内的点，还是想在广场上寻找更远的点。我个人建议，通过这个干部名单循环计算每个结果与起点的距离，并按计算的距离对结果进行排序。

在第三步中，你可以使用一个简单的过滤器来删除与定义的圆圈超过所需距离的位置。
