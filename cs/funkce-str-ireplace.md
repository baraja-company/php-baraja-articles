PHP funkce str_ireplace()
=========================

> id: e345edcc-0c33-4cf0-b99d-e14898a4e564
> slug:
> 	cs: funkce-str-ireplace
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.0`

Case-insensitive version of <function>str_replace</function>.


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$search` | `mixed` |  | Every replacement with search array is performed on the result of previous replacement. |
| `$replace` | `mixed` |  |  |
| `$subject` | `mixed` |  | If subject is an array, then the search and replace is performed with every entry of subject, and the return value is an array as well. |
| `$count` | `int` | null | The number of matched and replaced needles will be returned in count which is passed by reference. |


Návratové hodnoty
----------------

`mixed`

a string or an array of replacements.

Další zdroje
------------

https://www.php.net/manual/en/function.str-ireplace.php
