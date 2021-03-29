PHP funkce is_dir()
===================

> id: "257cc0c1-c7da-4f9b-8c4a-21ecaba5dafd"
> slugCS: funkce-is-dir
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Tells whether the filename is a directory


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` |  | Path to the file. If filename is a relative filename, it will be checked relative to the current working directory. If filename is a symbolic or hard link then the link will be resolved and checked. |


Návratové hodnoty
----------------

`bool`

true if the filename exists and is a directory, false
otherwise.

Další zdroje
------------

https://php.net/manual/en/function.is-dir.php
