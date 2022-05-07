Koniec()
========

> id: '879c7c9a-a497-4080-865d-f15e6c9e8578'
> slug:
> 	cs: end
> 	sk: koniec
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '2d097dd1fae4e4f125d879c92bc2cfbe'

- Podpora PHP 4, PHP 5
- Krátky popis: nastaví vnútorný ukazovateľ poľa na jeho posledný prvok.
- Požiadavky.

Popis
--------------------------

Funkcia `end()` nastaví vnútorný ukazovateľ poľa na posledný prvok a vráti jeho hodnotu.

Podobné funkcie
--------------------------

- `current()`
- `each()`
- `prev()`
- <a href="/reset">reset()</a>
- `next()`

Príklad
--------------------------

```php
echo end([
    'jablko',
    'banán',
    'brusnice',
]); // vypíše brusnice
```



```php
$a = [];
$a[1] = 1;
$a[0] = 0;

echo end($a); // vytlačí 0
```

Parametre
--------------------------

| # | Typ | Popis |
| --- | ------- | ----- |
| 1 | `array` | Názov poľa, s ktorým sa pracuje. |

Vrátené hodnoty
--------------------------

Vráti hodnotu posledného prvku alebo `false` pre prázdne pole.

Rozdiely oproti predchádzajúcim verziám
--------------------------

Žiadne
