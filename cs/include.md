PHP Include - vložení souboru do stránky
========================================

> id: "7a53145c-8552-425d-b864-283f73a7a7de"
> slug:
> 	cs: include
> 
> publicationDate: "2019-09-11 10:07:07"
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f

Konstrukt `include` automaticky vkládá do aktuální stránky další soubory/script.

Pokud jde o PHP, bude automaticky spuštěn a pochopen tak, jako kdyby na uvedeném místě vždycky byl.

Příklad:

```php
include 'novinky.html';
```

Praktické využití
-----------------

- Společné menu a nabídky,
- Novinky, aktuality, zprávy a stejné obsahy,
- Hlavička nebo patička, ...

Dynamické načítání souborů
--------------------------

Často potřebujeme soubory načítat dynamicky, například na základě proměnné.

Například:

```php
$clanek = 'neco';

include $clanek . '.html';
```

Nebo můžeme do stránky dynamicky načítat články:

```php
include 'clanky/' . $_GET['page'] . '.html';
```

Po zavolání URL s parametrem `page` dojde k automatickému vložení článku, například: `https://example.com/?page=novinky`.
