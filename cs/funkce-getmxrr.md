PHP funkce getmxrr()
====================

> id: "95f29209-27d7-4514-9488-30e26b3fdabc"
> slugCS: funkce-getmxrr
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Get MX records corresponding to a given Internet host name


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$hostname` | `string` |  | The Internet host name. |
| `$mxhosts` | `array` |  | A list of the MX records found is placed into the array mxhosts. |
| `$weight` | `array` | null | If the weight array is given, it will be filled with the weight information gathered. |


Návratové hodnoty
----------------

`bool`

true if any records are found; returns false if no records
were found or if an error occurred.

Další zdroje
------------

https://php.net/manual/en/function.getmxrr.php
