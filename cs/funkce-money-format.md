PHP funkce money_format()
=========================

> id: eb7649d0-1509-45f6-a35a-33812fe68758
> slug:
> 	cs: funkce-money-format
> 
> publicationDate: "2019-09-11 10:04:00"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.3.0`

Formats a number as a currency string


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$format` | `string` |  | The format specification consists of the following sequence: <p>a % character |
| `$number` | `float` |  | The number to be formatted. |


Návratové hodnoty
----------------

`string`

the formatted string. Characters before and after the formatting
string will be returned unchanged.
Non-numeric number causes returning &null; and
emitting E_WARNING.

Další zdroje
------------

https://php.net/manual/en/function.money-format.php
