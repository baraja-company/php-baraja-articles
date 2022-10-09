PHP Include - indsættelse af en fil i en side
=============================================

> id: '7a53145c-8552-425d-b864-283f73a7a7de'
> slug:
> 	cs: include
> 	da: php-include-indsaettelse-af-en-fil-i-en-side
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '6e18e67ec03b3a7592473230559fc364'

Konstruktionen `include` indsætter automatisk yderligere filer/skripter i den aktuelle side.

For PHP's vedkommende vil den automatisk blive udført og forstået, som om den altid har været på denne placering.

Eksempel:

```php
include 'news.html';
```

Praktisk anvendelse
-----------------

- Fælles menuer og menuer,
- Nyheder, opdateringer, nyheder, nyheder og det samme indhold,
- Overskrift eller sidefod, ...

Dynamisk indlæsning af filer
--------------------------

Ofte har vi brug for at indlæse filer dynamisk, f.eks. baseret på en variabel.

For eksempel:

```php
$clanek = 'neco';

include $clanek . '.html';
```

Eller vi kan indlæse artikler dynamisk på siden:

```php
include 'artikler/' . $_GET['side'] . '.html';
```

Når en URL med parameteren `page` kaldes, indsættes artiklen automatisk, f.eks. `https://example.com/?page=novinky`.
