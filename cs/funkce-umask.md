PHP funkce umask()
==================

> id: a8367c75-5190-4055-a7bd-6e37ade7020f
> slugCS: funkce-umask
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Changes the current umask


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$mask` | `int` | null | The new umask. |


Návratové hodnoty
----------------

`int`

umask without arguments simply returns the
current umask otherwise the old umask is returned.

Další zdroje
------------

https://php.net/manual/en/function.umask.php
