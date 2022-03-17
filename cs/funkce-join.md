PHP funkce join()
=================

> id: "8f545dfa-4525-47ca-b291-d694747f8b03"
> slug:
> 	cs: funkce-join
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

&Alias; <function>implode</function>


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$glue` | `string` | "", | Defaults to an empty string. This is not the preferred usage of implode as glue would be the second parameter and thus, the bad prototype would be used. |
| `$pieces` | `array` | *není* | The array of strings to implode. |


Návratové hodnoty
----------------

`string`

a string containing a string representation of all the array
elements in the same order, with the glue string between each element.

Další zdroje
------------

https://www.php.net/manual/en/function.join.php
