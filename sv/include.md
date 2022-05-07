PHP Include - infoga en fil på en sida
======================================

> id: '7a53145c-8552-425d-b864-283f73a7a7de'
> slug:
> 	cs: include
> 	sv: php-include---infoga-en-fil-pa-en-sida
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '6e18e67ec03b3a7592473230559fc364'

Konstruktionen `include` infogar automatiskt ytterligare filer/skript i den aktuella sidan.

När det gäller PHP kommer det att utföras automatiskt och förstås som om det alltid hade funnits på den platsen.

Exempel:

```php
include 'news.html';
```

Praktisk användning
-----------------

- Gemensamma menyer och menyer,
- Nyheter, uppdateringar, nyheter och samma innehåll,
- Rubrik eller sidfot, ...

Dynamisk filinläsning
--------------------------

Ofta behöver vi ladda filer dynamiskt, till exempel baserat på en variabel.

Till exempel:

```php
$clanek = 'neco';

include $clanek . '.html';
```

Eller så kan vi dynamiskt läsa in artiklar på sidan:

```php
include 'artiklar/' . $_GET['sidan'] . '.html';
```

När en URL med parametern `page` anropas infogas artikeln automatiskt, till exempel: `https://example.com/?page=novinky`.
