PHP funkcia array_walk_recursive()
==================================

> id: e71c1de4-4333-4d8d-9160-8c0d30481f46
> slug:
> 	cs: funkce-array-walk-recursive
> 	sk: php-funkcia-array-walk-recursive
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '59874540-664b-4474-9869-7e6742ab6051'
> sourceContentHash: c3bfa9053e0fdaf823eea6a4da6ecb81

Dostupnosť v `PHP 5.0`

rekurzívne použitie používateľskej funkcie na každý člen poľa


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$input` | `array` | *not* | Vstupné pole. |
| `$funcname` | `callback` | *not* | Typicky má funcname dva parametre. Hodnota vstupného parametra je prvá a kľúč/index druhý. |
| `$userdata` | `mixed` | null | Ak je zadaný nepovinný parameter userdata, bude odovzdaný ako tretí parameter spätnej funkcie callname. |


Vrátené hodnoty
----------------

`bool`

Vráti `true` pri úspechu, inak `false` pri neúspechu.

Ďalšie zdroje
------------

[Oficiálna dokumentácia k funkcii array-walk-recursive](https://www.php.net/manual/en/function.array-walk-recursive.php)
