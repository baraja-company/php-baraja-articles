PHP funkce strrchr()
====================

> id: "1d467cb8-f0f5-40a0-900a-e2591eb9605e"
> slugCS: funkce-strrchr
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Find the last occurrence of a character in a string


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$haystack` | `string` |  | The string to search in |
| `$needle` | `mixed` |  | If <b>needle</b> contains more than one character, only the first is used. This behavior is different from that of {@see strstr()}. |


Návratové hodnoty
----------------

`string`

<p>
This function returns the portion of string, or <b>FALSE</b> if
<b>needle</b> is not found.
</p>

Další zdroje
------------

https://php.net/manual/en/function.strrchr.php
