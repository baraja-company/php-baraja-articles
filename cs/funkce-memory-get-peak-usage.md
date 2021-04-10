PHP funkce memory_get_peak_usage()
==================================

> id: a3a5f2b2-21e3-4d2e-8c1d-3a8af523b652
> slug:
> 	cs: funkce-memory-get-peak-usage
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: ab12b253-41a0-4bf3-9019-956608d7d534

Dostupnost ve verzích: `PHP 5.2.0`

Returns the peak of memory allocated by PHP


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$real_usage` | `bool` | null | Set this to true to get the real size of memory allocated from system. If not set or false only the memory used by emalloc() is reported. |


Návratové hodnoty
----------------

`int`

the memory peak in bytes.

Další zdroje
------------

https://php.net/manual/en/function.memory-get-peak-usage.php
