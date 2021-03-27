PHP funkce popen()
================================

> id: b49b0002-c900-4112-804e-89e046dd5de6
> slugCS: funkce-popen
> publicationDate: 2019-09-11 10:04:04
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 4.0`

Opens process file pointer


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$command` | `string` |  | The command |
| `$mode` | `string` |  | The mode |


Návratové hodnoty
----------------

`resource`

a file pointer identical to that returned by
fopen, except that it is unidirectional (may
only be used for reading or writing) and must be closed with
pclose. This pointer may be used with
fgets, fgetss, and
fwrite.
</p>
<p>
If an error occurs, returns false.

Další zdroje
------------

https://php.net/manual/en/function.popen.php
