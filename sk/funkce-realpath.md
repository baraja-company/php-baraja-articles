PHP funkcia realpath()
======================

> id: '28a7eaf9-5c2e-4933-b701-5637ea0857ab'
> slug:
> 	cs: funkce-realpath
> 	sk: php-funkcia-realpath
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: ca3ae593548775332eceff3ff99accaa

Dostupnosť v `PHP 4.0`

Vracia kanonizované absolútne meno cesty


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$path` | `string` | *not* | Kontrolovaná cesta. |


Vrátené hodnoty
----------------

`string`

|bool kanonizovaný absolútny názov cesty v prípade úspechu. Výsledná cesta
nebude mať žiadne symbolické prepojenie, komponenty '/./' alebo '/../'.
</p>
<p>
realpath vráti false pri zlyhaní, napr. ak
súbor neexistuje.

Ďalšie zdroje
------------

[Oficiálna dokumentácia realpath](https://www.php.net/manual/en/function.realpath.php)
