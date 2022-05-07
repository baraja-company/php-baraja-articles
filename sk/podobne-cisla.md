Podobné čísla - ako ich rozpoznať
=================================

> id: d1d911d1-a6cd-4a69-af17-e90ad1d95b6d
> slug:
> 	cs: podobne-cisla
> 	sk: podobne-cisla---ako-ich-rozpoznat
> 
> publicationDate: '2019-08-23 15:49:48'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '07805228d14d5a91ba57a86914c3c8dd'

V minulosti boli v tomto článku opísané metódy na rozpoznávanie podobných čísel.

Napríklad `500 199 Kč` a `500 210 Kč` sú takmer rovnaké.

Riešením je vypočítať podiel a porovnať ho s epsilonom.

```php
$x = 500199;
$y = 500210;
$epsilon = 0.001;

if (abs($x / $y) < $epsilon) {
    // Čísla sú podobné
}
```
