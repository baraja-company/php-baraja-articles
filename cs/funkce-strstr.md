PHP funkce strstr()
===================

> id: "817c2566-9e01-4b1f-b36a-8a04a8ec5dae"
> slug:
> 	cs: funkce-strstr
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Find first occurrence of a string


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$haystack` | `string` | *není* | The input string. |
| `$needle` | `mixed` | *není* | If needle is not a string, it is converted to an integer and applied as the ordinal value of a character. |
| `$before_needle` | `bool` | null | If true, strstr returns the part of the haystack before the first occurrence of the needle. |


Návratové hodnoty
----------------

`string`

the portion of string, or false if needle
is not found.

Další zdroje
------------

https://www.php.net/manual/en/function.strstr.php
