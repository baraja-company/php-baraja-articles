PHP funkcia readfile()
======================

> id: e98818bd-3b2f-4b36-983d-c3fd507d3ce3
> slug:
> 	cs: funkce-readfile
> 	sk: php-funkcia-readfile
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: f3a53f7a8fe7b586ed41e53505d6169c

Dostupnosť v `PHP 4.0`

Výstup súboru


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | Názov súboru, ktorý sa číta. |
| `$use_include_path` | `bool` | null, | Môžete použiť nepovinný druhý parameter a nastaviť ho na hodnotu true, ak chcete súbor hľadať aj v include_path. |
| `$context` | `resource` | null | Zdroj kontextového toku. |


Vrátené hodnoty
----------------

`int`

počet bajtov načítaných zo súboru. Ak sa vyskytne chyba
sa vráti false a pokiaľ funkcia nebola volaná ako

Ďalšie zdroje
------------

[Oficiálna dokumentácia funkcie readfile](https://www.php.net/manual/en/function.readfile.php)
