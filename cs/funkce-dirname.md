PHP funkce dirname()
====================

> id: "5e3cd7db-939c-4a11-a7c7-f87faaf92b77"
> slug:
> 	cs: funkce-dirname
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Returns directory name component of path


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$path` | `string` | *není* | A path. |
| `$levels` | `int` | 1 | The number of parent directories to go up. This must be an integer greater than 0. |


Návratové hodnoty
----------------

`string`

the name of the directory. If there are no slashes in
path, a dot ('.') is returned,
indicating the current directory. Otherwise, the returned string is
path with any trailing
/component removed.

Další zdroje
------------

[Oficiální dokumentace funkce dirname](https://www.php.net/manual/en/function.dirname.php)
