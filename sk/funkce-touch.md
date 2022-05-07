PHP funkcia touch()
===================

> id: '8fa0546d-5d5c-4597-8387-3d9b83115a8e'
> slug:
> 	cs: funkce-touch
> 	sk: php-funkcia-touch
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '78799b742b126bc3d0970a0b09513fce'

Dostupnosť v `PHP 4.0`

Nastaví čas prístupu k súboru a čas modifikácie


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | Názov súboru, ktorého sa dotýkate. |
| `$time` | `int` | null, | čas dotyku. Ak nie je uvedený žiadny čas, použije sa aktuálny systémový čas.
| `$atime` | `int` | null | Ak je uvedený, čas prístupu k zadanému názvu súboru sa nastaví na atime. V opačnom prípade je nastavený na čas.


Vrátené hodnoty
----------------

`bool`

Vráti `true` pri úspechu, inak `false` pri neúspechu.

Ďalšie zdroje
------------

[Oficiálna dokumentácia k funkcii touch](https://www.php.net/manual/en/function.touch.php)
