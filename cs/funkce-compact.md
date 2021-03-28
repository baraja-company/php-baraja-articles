PHP funkce compact()
================================

> id: e3f2786a-c15e-4af1-b52f-52439d4e54c5
> slugCS: funkce-compact
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Create array containing variables and their values


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$varname` | `mixed` |  | compact takes a variable number of parameters. Each parameter can be either a string containing the name of the variable, or an array of variable names. The array can contain other arrays of variable names inside it; compact handles it recursively. |
| `$_` | `mixed` | null |  |


Návratové hodnoty
----------------

`array`

the output array with all the variables added to it.

Další zdroje
------------

https://php.net/manual/en/function.compact.php
