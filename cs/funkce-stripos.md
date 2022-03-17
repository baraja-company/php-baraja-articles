PHP funkce stripos()
====================

> id: "93bcf683-be6f-4cfa-84ee-69b04af69604"
> slug:
> 	cs: funkce-stripos
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.0`

Find position of first occurrence of a case-insensitive string


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$haystack` | `string` | *není* | The string to search in |
| `$needle` | `string` | *není* | Note that the needle may be a string of one or more characters. |
| `$offset` | `int` | null | The optional offset parameter allows you to specify which character in haystack to start searching. The position returned is still relative to the beginning of haystack. |


Návratové hodnoty
----------------

`int`

If needle is not found,
stripos will return boolean false.

Další zdroje
------------

https://www.php.net/manual/en/function.stripos.php
