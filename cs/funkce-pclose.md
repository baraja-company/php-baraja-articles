PHP funkce pclose()
===================

> id: dd52fa6f-c280-4639-a71c-665b0dcf7691
> slugCS: funkce-pclose
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Closes process file pointer


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$handle` | `resource` |  | The file pointer must be valid, and must have been returned by a successful call to popen. |


Návratové hodnoty
----------------

`int`

the termination status of the process that was run.

Další zdroje
------------

https://php.net/manual/en/function.pclose.php
