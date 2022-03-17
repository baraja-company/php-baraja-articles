PHP funkce iptcparse()
======================

> id: a768ebe8-827c-40cf-b56e-f5661e665091
> slug:
> 	cs: funkce-iptcparse
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Parse a binary IPTC block into single tags.


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$iptcblock` | `string` | *není* | A binary IPTC block. |


Návratové hodnoty
----------------

`array`

an array using the tagmarker as an index and the value as the
value. It returns false on error or if no IPTC data was found.

Další zdroje
------------

https://www.php.net/manual/en/function.iptcparse.php
