PHP funkce strspn()
================================

> id: 03d42f61-2887-452b-9be3-2ddda936c17d
> slugCS: funkce-strspn
> publicationDate: 2019-09-11 10:04:03
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 4.0`

Finds the length of the first segment of a string consisting


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$subject` | `string` |  | The string to examine. |
| `$mask` | `string` |  | The list of allowable characters to include in counted segments. |
| `$start` | `int` | null, | The position in subject to start searching. |
| `$length` | `int` | null | The length of the segment from subject to examine. |


Návratové hodnoty
----------------

`int`

the length of the initial segment of str1
which consists entirely of characters in str2.

Další zdroje
------------

https://php.net/manual/en/function.strspn.php
