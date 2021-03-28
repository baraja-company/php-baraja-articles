Podobná čísla - jak ho poznat
================================

> id: d1d911d1-a6cd-4a69-af17-e90ad1d95b6d
> slugCS: podobne-cisla
> publicationDate: "2019-08-23 15:49:48"
> mainCategoryId: "1f73dcfa-92a9-4738-ab30-8cbfb00ad23b"

Tento článek v minulosti popisoval metody, jak poznat podobná čísla.

Například `500 199 Kč` a `500 210 Kč` je skoro stejně.

Řešení je vypočítat podíl a porovnat vůči epsilon.

```php
$x = 500199;
$y = 500210;
$epsilon = 0.001;

if (abs($x / $y) < $epsilon) {
    // Čísla jsou podobná
}
```
