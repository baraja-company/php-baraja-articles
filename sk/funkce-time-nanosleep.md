PHP funkcia time_nanosleep()
============================

> id: '481733e0-3060-4750-b1e8-c3ef63988d0a'
> slug:
> 	cs: funkce-time-nanosleep
> 	sk: php-funkcia-time-nanosleep
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '7e194bfe898362d00aa75709dba27a68'

Dostupnosť v `PHP 5.0`

Oneskorenie v sekundách a nanosekundách


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$seconds` | `int` | *not* | Musí to byť kladné celé číslo. |
| `$nanoseconds` | `int` | *not* | Musí to byť kladné celé číslo menšie ako 1 miliarda. |


Vrátené hodnoty
----------------

`bool`

Vráti `true` pri úspechu, inak `false` pri neúspechu.

Ak bolo oneskorenie prerušené signálom, vráti sa asociatívne pole so zložkami:

- seconds - počet sekúnd, ktoré zostávajú do konca oneskorenia.
- nanosekundy - počet nanosekúnd zostávajúcich do oneskorenia

Ďalšie zdroje
------------

[Oficiálna dokumentácia funkcie časového nano-spánku](https://www.php.net/manual/en/function.time-nanosleep.php)
