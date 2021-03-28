PHP funkce count_chars()
================================

> id: e48cf8d3-a048-4e33-961e-aa6a04e122e7
> slugCS: funkce-count-chars
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Return information about characters used in a string


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$string` | `string` |  | The examined string. |
| `$mode` | `int` | null | See return values. |


Návratové hodnoty
----------------

`mixed`

Depending on mode
count_chars returns one of the following:
0 - an array with the byte-value as key and the frequency of
every byte as value.
1 - same as 0 but only byte-values with a frequency greater
than zero are listed.
2 - same as 0 but only byte-values with a frequency equal to
zero are listed.
3 - a string containing all unique characters is returned.
4 - a string containing all not used characters is returned.

Další zdroje
------------

https://php.net/manual/en/function.count-chars.php
