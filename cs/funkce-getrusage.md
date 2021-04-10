PHP funkce getrusage()
======================

> id: c19b63b6-232b-46f5-ade7-996308d5464f
> slug:
> 	cs: funkce-getrusage
> 
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Gets the current resource usages


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$who` | `int` | null | If who is 1, getrusage will be called with RUSAGE_CHILDREN. |


Návratové hodnoty
----------------

`array`

an associative array containing the data returned from the system
call. All entries are accessible by using their documented field names.

Další zdroje
------------

https://php.net/manual/en/function.getrusage.php
