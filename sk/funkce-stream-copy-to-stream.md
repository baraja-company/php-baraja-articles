PHP funkcia stream_copy_to_stream()
===================================

> id: '2a32e374-fa9c-4c8a-93dc-3b5eb907cdb5'
> slug:
> 	cs: funkce-stream-copy-to-stream
> 	sk: php-funkcia-stream-copy-to-stream
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: fafbde2983979d6b05143124fa436c79

Dostupnosť v `PHP 5.0`

Kopíruje údaje z jedného prúdu do druhého


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$source` | `resource` | *not* | Zdrojový prúd |
| `$dest` | `resource` | *not* | Cieľový prúd |
| `$maxlength` | `int` | null, | Maximálny počet bajtov na kopírovanie |
| `$offset` | `int` | null | Odstup, odkiaľ sa majú začať kopírovať údaje |


Vrátené hodnoty
----------------

`int`

celkový počet skopírovaných bajtov.

Ďalšie zdroje
------------

[Oficiálna dokumentácia pre kopírovanie zo streamu do streamu](https://www.php.net/manual/en/function.stream-copy-to-stream.php)
