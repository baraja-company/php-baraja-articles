PHP funkce print_r()
================================

> id: 91a2909b-d1ac-4180-b859-ef776ccbfcaf
> slugCS: funkce-print-r
> publicationDate: 2019-09-11 10:04:03
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 4.0`

Prints human-readable information about a variable


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$expression` | `mixed` |  | The expression to be printed. |
| `$return` | `bool` | null | If you would like to capture the output of print_r, use the return parameter. If this parameter is set to true, print_r will return its output, instead of printing it (which it does by default). |


Návratové hodnoty
----------------

`mixed`

If given a string, integer or float,
the value itself will be printed. If given an array, values
will be presented in a format that shows keys and elements. Similar
notation is used for objects.

Další zdroje
------------

https://php.net/manual/en/function.print-r.php
