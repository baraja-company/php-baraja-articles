PHP funkce round()
==================

> id: "7c38379c-cfd2-47ba-93e4-4e770809dfdd"
> slug:
> 	cs: funkce-round
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Returns the rounded value of val to specified precision (number of digits after the decimal point).
precision can also be negative or zero (default).
Note: PHP doesn't handle strings like "12,300.2" correctly by default. See converting from strings.


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$val` | `float` |  | The value to round |
| `$precision` | `int` | 0, | The optional number of decimal digits to round to. |
| `$mode` | `int` | PHP_ROUND_HALF_UP | One of PHP_ROUND_HALF_UP, PHP_ROUND_HALF_DOWN, PHP_ROUND_HALF_EVEN, or PHP_ROUND_HALF_ODD. |


Návratové hodnoty
----------------

`float`

The rounded value

Další zdroje
------------

https://php.net/manual/en/function.round.php
