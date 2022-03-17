PHP funkce socket_set_timeout()
===============================

> id: "3afe8aaf-5ad8-4929-b0ef-ab8ac6d66328"
> slug:
> 	cs: funkce-socket-set-timeout
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

&Alias; <function>stream_set_timeout</function>
<p>Set timeout period on a stream


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$stream` | `resource` | *není* | The target stream. |
| `$seconds` | `int` | *není* | The seconds part of the timeout to be set. |
| `$microseconds` | `int` | 0 | The microseconds part of the timeout to be set. |


Návratové hodnoty
----------------

`bool`

true on success or false on failure.

Další zdroje
------------

https://www.php.net/manual/en/function.socket-set-timeout.php
