PHP funkce intval()
================================

> id: 870a74d9-8dce-4897-8b14-ea35732e4f12
> slugCS: funkce-intval
> publicationDate: 2019-09-11 10:04:03
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 5.5.0
/
function boolval($var) {}

/**
Get the integer value of a variable`, `PHP 4.0`

(PHP 5.5.0)<br/>
Get the boolean value of a variable


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$var` | `mixed` |  | the scalar value being converted to a boolean. |
| `$base` | `int` | null | The base for the conversion |


Návratové hodnoty
----------------


- boolean The boolean value of var.
- int The integer value of var on success, or 0 on
failure. Empty arrays and objects return 0, non-empty arrays and
objects return 1.
</p>
<p>
The maximum value depends on the system. 32 bit systems have a
maximum signed integer range of -2147483648 to 2147483647. So for example
on such a system, intval('1000000000000') will return
2147483647. The maximum signed integer value for 64 bit systems is
9223372036854775807.
</p>
<p>
Strings will most likely return 0 although this depends on the
leftmost characters of the string. The common rules of
integer casting
apply.

Další zdroje
------------

https://php.net/manual/en/function.intval.php
