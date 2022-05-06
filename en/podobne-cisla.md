Similar numbers - how to recognize it
=====================================

> id: d1d911d1-a6cd-4a69-af17-e90ad1d95b6d
> slug:
> 	cs: podobne-cisla
> 	en: similar-numbers---how-to-recognize-it
> 
> publicationDate: '2019-08-23 15:49:48'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '07805228d14d5a91ba57a86914c3c8dd'

In the past, this article has described methods to recognize similar numbers.

For example, `500 199 Kč` and `500 210 Kč` are almost the same.

The solution is to calculate the proportion and compare against epsilon.

```php
$x = 500199;
$y = 500210;
$epsilon = 0.001;

if (abs($x / $y) < $epsilon) {
    // The numbers are similar
}
```
