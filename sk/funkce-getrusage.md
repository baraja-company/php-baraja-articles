PHP funkcia getrusage()
=======================

> id: c19b63b6-232b-46f5-ade7-996308d5464f
> slug:
> 	cs: funkce-getrusage
> 	sk: php-funkcia-getrusage
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '0faa1f709e037a48e694b2e0b229de5c'

Dostupnosť v `PHP 4.0`

Získava aktuálne využitie zdrojov


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$who` | `int` | null | Ak je who 1, getrusage sa zavolá s RUSAGE_CHILDREN. |


Vrátené hodnoty
----------------

`array`

asociatívne pole obsahujúce údaje vrátené zo systému
zavolajte. Všetky položky sú prístupné pomocou ich zdokumentovaných názvov polí.

Ďalšie zdroje
------------

[Oficiálna dokumentácia getrusage](https://www.php.net/manual/en/function.getrusage.php)
