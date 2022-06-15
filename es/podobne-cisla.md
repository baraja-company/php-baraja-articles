Números similares: cómo reconocerlos
====================================

> id: d1d911d1-a6cd-4a69-af17-e90ad1d95b6d
> slug:
> 	cs: podobne-cisla
> 	es: numeros-similares-como-reconocerlos
> 
> publicationDate: '2019-08-23 15:49:48'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '07805228d14d5a91ba57a86914c3c8dd'

En el pasado, este artículo ha descrito métodos para reconocer números similares.

Por ejemplo, `500 199 Kč` y `500 210 Kč` son casi iguales.

La solución es calcular la proporción y compararla con la épsilon.

```php
$x = 500199;
$y = 500210;
$epsilon = 0.001;

if (abs($x / $y) < $epsilon) {
    // Los números son similares
}
```
