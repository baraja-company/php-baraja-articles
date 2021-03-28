PHP funkce stristr()
================================

> id: "7966a56b-2add-421a-9bea-887e83a1ed71"
> slugCS: funkce-stristr
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Case-insensitive <function>strstr</function>


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$haystack` | `string` |  | The string to search in |
| `$needle` | `mixed` |  | If needle is not a string, it is converted to an integer and applied as the ordinal value of a character. |
| `$before_needle` | `bool` | null | If true, stristr returns the part of the haystack before the first occurrence of the needle. |


Návratové hodnoty
----------------

`string`

the matched substring. If needle is not
found, returns false.

Další zdroje
------------

https://php.net/manual/en/function.stristr.php
