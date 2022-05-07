PHP funkcia fileowner()
=======================

> id: '64e4aa1d-2b93-406f-982d-dc98ff1ab042'
> slug:
> 	cs: funkce-fileowner
> 	sk: php-funkcia-fileowner
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '761759ae7bedc13c577612c689b8e5cd'

Dostupnosť v `PHP 4.0`

Získa vlastníka súboru


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | Cesta k súboru. |


Vrátené hodnoty
----------------

`int`

|bool ID používateľa vlastníka súboru alebo false v prípade neúspechu.
ID používateľa sa vráti v číselnom formáte, použite
posix_getpwuid, aby ste ho prepočítali na používateľské meno.

Ďalšie zdroje
------------

[Oficiálna dokumentácia vlastníka súborov](https://www.php.net/manual/en/function.fileowner.php)
