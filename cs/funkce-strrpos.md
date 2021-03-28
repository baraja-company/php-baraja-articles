PHP funkce strrpos()
================================

> id: c2d989c9-94bc-4f21-838a-b9465065d4a9
> slugCS: funkce-strrpos
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Find the position of the last occurrence of a substring in a string


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$haystack` | `string` |  | The string to search in. |
| `$needle` | `string` |  | If <b>needle</b> is not a string, it is converted to an integer and applied as the ordinal value of a character. |
| `$offset` | `int` | 0 | If specified, search will start this number of characters counted from the beginning of the string. If the value is negative, search will instead start from that many characters from the end of the string, searching backwards. |


Návratové hodnoty
----------------

`int`

|boolean <p>
Returns the position where the needle exists relative to the beginning of
the <b>haystack</b> string (independent of search direction
or offset).
Also note that string positions start at 0, and not 1.
</p>
<p>
Returns <b>FALSE</b> if the needle was not found.
</p>

Další zdroje
------------

https://php.net/manual/en/function.strrpos.php
