PHP funkce sha1()
=================

> id: c78c5fb5-5ab4-4fac-b044-995d3306aaef
> slug:
> 	cs: funkce-sha1
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.3.0`

Calculate the sha1 hash of a string


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$str` | `string` | *není* | The input string. |
| `$raw_output` | `bool` | null | If the optional raw_output is set to true, then the sha1 digest is instead returned in raw binary format with a length of 20, otherwise the returned value is a 40-character hexadecimal number. |


Návratové hodnoty
----------------

`string`

the sha1 hash as a string.

Další zdroje
------------

https://www.php.net/manual/en/function.sha1.php
