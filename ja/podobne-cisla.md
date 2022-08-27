似たような数字 - 見分け方
==============

> id: d1d911d1-a6cd-4a69-af17-e90ad1d95b6d
> slug:
> 	cs: podobne-cisla
> 	ja: shitayouna-shu-zi-jian-fenke-fang
> 
> publicationDate: '2019-08-23 15:49:48'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '07805228d14d5a91ba57a86914c3c8dd'

過去にこの記事では、似たような数字を認識する方法を紹介したことがある。

例えば、`500 199 Kč`と`500 210 Kč`はほぼ同じです。

解決策は、比率を計算し、εと比較することです。

```php
$x = 500199;
$y = 500210;
$epsilon = 0.001;

if (abs($x / $y) < $epsilon) {
    // 数字は似ている
}
```
