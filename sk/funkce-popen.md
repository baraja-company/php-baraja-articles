PHP funkcia popen()
===================

> id: b49b0002-c900-4112-804e-89e046dd5de6
> slug:
> 	cs: funkce-popen
> 	sk: php-funkcia-popen
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '937c46146ce7ac71a4c0b0668745bf79'

Dostupnosť v `PHP 4.0`

Otvorí ukazovateľ na súbor procesu


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$command` | `string` | *not* | Príkaz |
| `$mode` | `string` | *not* | Režim |


Vrátené hodnoty
----------------

`resource`

ukazovateľ na súbor totožný s ukazovateľom vráteným pomocou
fopen, s tým rozdielom, že je jednosmerný (môže
používať len na čítanie alebo zápis) a musia byť uzavreté pomocou
pclose. Tento ukazovateľ sa môže používať s
fgets, fgetss a
fwrite.
</p>
<p>
Ak nastane chyba, vráti false.

Ďalšie zdroje
------------

[Oficiálna dokumentácia popenu](https://www.php.net/manual/en/function.popen.php)
