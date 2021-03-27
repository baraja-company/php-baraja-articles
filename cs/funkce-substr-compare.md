PHP funkce substr_compare()
================================

> id: 67b0a6e5-9779-4326-a489-53e114b777a5
> slugCS: funkce-substr-compare
> publicationDate: 2019-09-11 10:04:03
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 5.0`

Binary safe comparison of 2 strings from an offset, up to length characters


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$main_str` | `string` |  | The main string being compared. |
| `$str` | `string` |  | The secondary string being compared. |
| `$offset` | `int` |  | The start position for the comparison. If negative, it starts counting from the end of the string. |
| `$length` | `int` | null, | The length of the comparison. |
| `$case_insensitivity` | `bool` | null | If case_insensitivity is true, comparison is case insensitive. |


Návratové hodnoty
----------------

`int`

&lt; 0 if main_str from position
offset is less than str, &gt;
0 if it is greater than str, and 0 if they are equal.
If offset is equal to or greater than the length of
main_str or length is set and
is less than 1, substr_compare prints a warning and returns
false.

Další zdroje
------------

https://php.net/manual/en/function.substr-compare.php
