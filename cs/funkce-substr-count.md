PHP funkce substr_count()
=========================

> id: fc7bdcab-0040-4c03-9923-1dbfabc6e503
> slug:
> 	cs: funkce-substr-count
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Count the number of substring occurrences


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$haystack` | `string` | *není* | The string to search in |
| `$needle` | `string` | *není* | The substring to search for |
| `$offset` | `int` | null, | The offset where to start counting |
| `$length` | `int` | null | The maximum length after the specified offset to search for the substring. It outputs a warning if the offset plus the length is greater than the haystack length. |


Návratové hodnoty
----------------

`int`

This functions returns an integer.

Další zdroje
------------

https://www.php.net/manual/en/function.substr-count.php
