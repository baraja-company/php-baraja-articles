Lignende tal - hvordan man genkender det
========================================

> id: d1d911d1-a6cd-4a69-af17-e90ad1d95b6d
> slug:
> 	cs: podobne-cisla
> 	da: lignende-tal-hvordan-man-genkender-det
> 
> publicationDate: '2019-08-23 15:49:48'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '07805228d14d5a91ba57a86914c3c8dd'

Tidligere har denne artikel beskrevet metoder til at genkende lignende tal.

F.eks. er `500 199 Kč` og `500 210 Kč` næsten det samme.

Løsningen er at beregne andelen og sammenligne den med epsilon.

```php
$x = 500199;
$y = 500210;
$epsilon = 0.001;

if (abs($x / $y) < $epsilon) {
    // Tallene er ens
}
```
