PHP funkce time_nanosleep()
===========================

> id: "481733e0-3060-4750-b1e8-c3ef63988d0a"
> slug:
> 	cs: funkce-time-nanosleep
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.0`

Delay for a number of seconds and nanoseconds


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$seconds` | `int` | *není* | Must be a positive integer. |
| `$nanoseconds` | `int` | *není* | Must be a positive integer less than 1 billion. |


Návratové hodnoty
----------------

`bool`

|array true on success or false on failure.
</p>
<p>
If the delay was interrupted by a signal, an associative array will be
returned with the components:
seconds - number of seconds remaining in
the delay
nanoseconds - number of nanoseconds
remaining in the delay

Další zdroje
------------

https://www.php.net/manual/en/function.time-nanosleep.php
