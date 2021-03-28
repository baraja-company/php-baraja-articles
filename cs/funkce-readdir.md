PHP funkce readdir()
================================

> id: "3f7538f0-d1f2-4938-a58e-0086cba907b3"
> slugCS: funkce-readdir
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Read entry from directory handle


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$dir_handle` | `resource` | null | The directory handle resource previously opened with opendir. If the directory handle is not specified, the last link opened by opendir is assumed. |


Návratové hodnoty
----------------

`string`

|bool the filename on success or false on failure.

Další zdroje
------------

https://php.net/manual/en/function.readdir.php
