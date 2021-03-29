PHP funkce ignore_user_abort()
==============================

> id: "87e66193-e092-4b60-88bd-c096b1206983"
> slugCS: funkce-ignore-user-abort
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Set whether a client disconnect should abort script execution


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$value` | `string` | null | If set, this function will set the ignore_user_abort ini setting to the given value. If not, this function will only return the previous setting without changing it. |


Návratové hodnoty
----------------

`int`

the previous setting, as an integer.

Další zdroje
------------

https://php.net/manual/en/function.ignore-user-abort.php
