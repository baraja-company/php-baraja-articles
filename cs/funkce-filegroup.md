PHP funkce filegroup()
================================

> id: 6429ca80-161b-4299-9a0b-e7ac4b715923
> slugCS: funkce-filegroup
> publicationDate: 2019-09-11 10:04:03
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 4.0`

Gets file group


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` |  | Path to the file. |


Návratové hodnoty
----------------

`int`

|bool the group ID of the file, or false in case
of an error. The group ID is returned in numerical format, use
posix_getgrgid to resolve it to a group name.
Upon failure, false is returned.

Další zdroje
------------

https://php.net/manual/en/function.filegroup.php
