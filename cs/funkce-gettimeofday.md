PHP funkce gettimeofday()
================================

> id: 30d09f0c-d4ea-4717-9d52-a674c79f0342
> slugCS: funkce-gettimeofday
> publicationDate: 2019-09-11 10:04:03
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 4.0`

Get current time


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$return_float` | `bool` | null | When set to true, a float instead of an array is returned. |


Návratové hodnoty
----------------

`mixed`

By default an array is returned. If return_float
is set, then a float is returned.
</p>
<p>
Array keys:
"sec" - seconds since the Unix Epoch
"usec" - microseconds
"minuteswest" - minutes west of Greenwich
"dsttime" - type of dst correction

Další zdroje
------------

https://php.net/manual/en/function.gettimeofday.php
