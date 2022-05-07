Numéros similaires - comment les reconnaître
============================================

> id: d1d911d1-a6cd-4a69-af17-e90ad1d95b6d
> slug:
> 	cs: podobne-cisla
> 	fr: numeros-similaires---comment-les-reconnaitre
> 
> publicationDate: '2019-08-23 15:49:48'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '07805228d14d5a91ba57a86914c3c8dd'

Dans le passé, cet article a décrit des méthodes permettant de reconnaître des nombres similaires.

Par exemple, `500 199 Kč` et `500 210 Kč` sont presque identiques.

La solution consiste à calculer la proportion et à la comparer à epsilon.

```php
$x = 500199;
$y = 500210;
$epsilon = 0.001;

if (abs($x / $y) < $epsilon) {
    // Les chiffres sont similaires
}
```
