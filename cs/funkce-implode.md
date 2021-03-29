PHP funkce implode()
====================

> id: e69cb651-79da-43ec-81fb-11afa2a19c77
> slugCS: funkce-implode
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Join array elements with a string


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$glue` | `string` | "", | Defaults to an empty string. This is not the preferred usage of implode as glue would be the second parameter and thus, the bad prototype would be used. |
| `$pieces` | `array` |  | The array of strings to implode. |


Návratové hodnoty
----------------

`string`

a string containing a string representation of all the array
elements in the same order, with the glue string between each element.

Další zdroje
------------

https://php.net/manual/en/function.implode.php
