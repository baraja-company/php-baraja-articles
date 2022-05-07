Numeri simili - come riconoscerli
=================================

> id: d1d911d1-a6cd-4a69-af17-e90ad1d95b6d
> slug:
> 	cs: podobne-cisla
> 	it: numeri-simili---come-riconoscerli
> 
> publicationDate: '2019-08-23 15:49:48'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '07805228d14d5a91ba57a86914c3c8dd'

In passato, questo articolo ha descritto metodi per riconoscere numeri simili.

Per esempio, `500 199 Kč` e `500 210 Kč` sono quasi uguali.

La soluzione è calcolare la proporzione e confrontarla con l'epsilon.

```php
$x = 500199;
$y = 500210;
$epsilon = 0.001;

if (abs($x / $y) < $epsilon) {
    // I numeri sono simili
}
```
