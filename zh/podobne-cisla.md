相似的数字--如何认识它
============

> id: d1d911d1-a6cd-4a69-af17-e90ad1d95b6d
> slug:
> 	cs: podobne-cisla
> 	zh: xiang-shi-de-shu-zi-ru-he-ren-shi-ta
> 
> publicationDate: '2019-08-23 15:49:48'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '07805228d14d5a91ba57a86914c3c8dd'

在过去，这篇文章介绍了识别类似数字的方法。

例如，"500 199 Kč "和 "500 210 Kč "几乎相同。

解决办法是计算比例并与epsilon进行比较。

```php
$x = 500199;
$y = 500210;
$epsilon = 0.001;

if (abs($x / $y) < $epsilon) {
    // 这些数字是相似的
}
```
