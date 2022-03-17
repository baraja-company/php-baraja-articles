PHP funkce range()
==================

> id: b8a6d78a-2b24-40c2-96ac-fb1f999ad340
> slug:
> 	cs: funkce-range
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Create an array containing a range of elements


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$low` | `mixed` | *není* | Low value. |
| `$high` | `mixed` | *není* | High value. |
| `$step` | `number` | null | If a step value is given, it will be used as the increment between elements in the sequence. step should be given as a positive number. If not specified, step will default to 1. |


Návratové hodnoty
----------------

`array`

an array of elements from low to
high, inclusive. If low > high, the sequence will
be from high to low.

Další zdroje
------------

https://www.php.net/manual/en/function.range.php
