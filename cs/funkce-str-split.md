PHP funkce str_split()
================================

> id: 32f08691-047b-4b91-b626-e3ba88a4ba79
> slugCS: funkce-str-split
> publicationDate: 2019-09-11 10:04:03
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 5.0`

Convert a string to an array


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$string` | `string` |  | The input string. |
| `$split_length` | `int` | 1 | Maximum length of the chunk. |


Návratové hodnoty
----------------

`array`

If the optional split_length parameter is
specified, the returned array will be broken down into chunks with each
being split_length in length, otherwise each chunk
will be one character in length.
</p>
<p>
false is returned if split_length is less than 1.
If the split_length length exceeds the length of
string, the entire string is returned as the first
(and only) array element.

Další zdroje
------------

https://php.net/manual/en/function.str-split.php
