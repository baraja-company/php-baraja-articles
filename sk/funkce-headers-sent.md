PHP funkcia headers_sent()
==========================

> id: '3cf42d2b-7dcd-418d-a5d6-d3674a8aa23c'
> slug:
> 	cs: funkce-headers-sent
> 	sk: php-funkcia-headers-sent
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '8d0235d6393d5bb8fa7c2651a4899ebb'

Dostupnosť v `PHP 4.0`

kontroluje, či alebo kam boli odoslané hlavičky


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$file` | `string` | null, | Ak sú nastavené nepovinné parametre súbor a riadok, headers_sent vloží do premenných súbor a riadok názov zdrojového súboru PHP a číslo riadku, na ktorom začal výstup.
| `$line` | `int` | null | Číslo riadku, na ktorom začal výstup. |


Vrátené hodnoty
----------------

`bool`

headers_sent vráti false, ak nie sú žiadne hlavičky HTTP
už boli odoslané, alebo inak pravdivé.

Ďalšie zdroje
------------

[Oficiálna dokumentácia k záhlavím](https://www.php.net/manual/en/function.headers-sent.php)
