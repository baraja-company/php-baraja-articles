PHP funkcia array_walk()
========================

> id: '9a37b2cc-6123-404b-ad12-8c430cbf1f14'
> slug:
> 	cs: funkce-array-walk
> 	sk: php-funkcia-array-walk
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '59874540-664b-4474-9869-7e6742ab6051'
> sourceContentHash: '7e8f4294405b7cb47fd238b03e4d5740'

Dostupnosť v `PHP 4.0`

Použitie používateľskej funkcie na každý člen poľa


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$array` | `array` | *not* | Vstupné pole. |
| `$funcname` | `callback` | *not* | Typicky má funcname dva parametre. Hodnota parametra poľa je prvá a kľúč/index druhý. |
| `$userdata` | `mixed` | null | Ak je zadaný nepovinný parameter userdata, bude odovzdaný ako tretí parameter spätnej funkcie callname. |


Vrátené hodnoty
----------------

`bool`

Vráti `true` pri úspechu, inak `false` pri neúspechu.

Ďalšie zdroje
------------

[Oficiálna dokumentácia funkcie array-walk](https://www.php.net/manual/en/function.array-walk.php)
