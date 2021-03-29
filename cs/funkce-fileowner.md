PHP funkce fileowner()
======================

> id: "64e4aa1d-2b93-406f-982d-dc98ff1ab042"
> slugCS: funkce-fileowner
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Gets file owner


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` |  | Path to the file. |


Návratové hodnoty
----------------

`int`

|bool the user ID of the owner of the file, or false on failure.
The user ID is returned in numerical format, use
posix_getpwuid to resolve it to a username.

Další zdroje
------------

https://php.net/manual/en/function.fileowner.php
