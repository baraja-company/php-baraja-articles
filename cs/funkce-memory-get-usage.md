PHP funkce memory_get_usage()
=============================

> id: fdf5801a-0d01-457d-ba99-8994201acdb2
> slug:
> 	cs: funkce-memory-get-usage
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: ab12b253-41a0-4bf3-9019-956608d7d534

Dostupnost ve verzích: `PHP 4.3.2`

Returns the amount of memory allocated to PHP


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$real_usage` | `bool` | null | Set this to true to get the real size of memory allocated from system. If not set or false only the memory used by emalloc() is reported. |


Návratové hodnoty
----------------

`int`

the memory amount in bytes.

Další zdroje
------------

https://php.net/manual/en/function.memory-get-usage.php
