PHP funkce stream_set_timeout()
===============================

> id: a6163d2c-53e7-43e6-b10c-962f09f52095
> slugCS: funkce-stream-set-timeout
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.3.0`

Set timeout period on a stream


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$stream` | `resource` |  | The target stream. |
| `$seconds` | `int` |  | The seconds part of the timeout to be set. |
| `$microseconds` | `int` | null | The microseconds part of the timeout to be set. |


Návratové hodnoty
----------------

`bool`

true on success or false on failure.

Další zdroje
------------

https://php.net/manual/en/function.stream-set-timeout.php
