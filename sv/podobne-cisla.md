Liknande nummer - hur man känner igen det
=========================================

> id: d1d911d1-a6cd-4a69-af17-e90ad1d95b6d
> slug:
> 	cs: podobne-cisla
> 	sv: liknande-nummer---hur-man-kaenner-igen-det
> 
> publicationDate: '2019-08-23 15:49:48'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '07805228d14d5a91ba57a86914c3c8dd'

Tidigare har vi i denna artikel beskrivit metoder för att känna igen liknande tal.

Exempelvis är "500 199 Kč" och "500 210 Kč" nästan identiska.

Lösningen är att beräkna andelen och jämföra med epsilon.

```php
$x = 500199;
$y = 500210;
$epsilon = 0.001;

if (abs($x / $y) < $epsilon) {
    // Siffrorna är likartade
}
```
