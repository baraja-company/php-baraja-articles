PHP Include - vloženie súboru do stránky
========================================

> id: '7a53145c-8552-425d-b864-283f73a7a7de'
> slug:
> 	cs: include
> 	sk: php-include---vlozenie-suboru-do-stranky
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '6e18e67ec03b3a7592473230559fc364'

Konštrukcia `include` automaticky vloží ďalšie súbory/skripty do aktuálnej stránky.

Pokiaľ ide o PHP, bude sa automaticky vykonávať a chápať tak, ako keby bol vždy na tomto mieste.

Príklad:

```php
include 'news.html';
```

Praktické využitie
-----------------

- Spoločné ponuky a menu,
- Novinky, aktualizácie, správy a rovnaký obsah,
- Záhlavie alebo päta, ...

Dynamické načítanie súborov
--------------------------

Často potrebujeme načítať súbory dynamicky, napríklad na základe premennej.

Napríklad:

```php
$clanek = 'neco';

include $clanek . '.html';
```

Alebo môžeme články na stránku načítať dynamicky:

```php
include 'articles/' . $_GET['page'] . '.html';
```

Vyvolanie adresy URL s parametrom `page` automaticky vloží článok, napríklad: `https://example.com/?page=novinky`.
