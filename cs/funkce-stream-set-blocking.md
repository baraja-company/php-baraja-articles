PHP funkce stream_set_blocking()
================================

> id: "1b06582b-7e6d-4a2e-bf35-ead2ccc2cb76"
> slug:
> 	cs: funkce-stream-set-blocking
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.3.0`

Set blocking/non-blocking mode on a stream


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$stream` | `resource` |  | The stream. |
| `$mode` | `int` |  | If mode is 0, the given stream will be switched to non-blocking mode, and if 1, it will be switched to blocking mode. This affects calls like fgets and fread that read from the stream. In non-blocking mode an fgets call will always return right away while in blocking mode it will wait for data to become available on the stream. |


Návratové hodnoty
----------------

`bool`

true on success or false on failure.

Další zdroje
------------

https://www.php.net/manual/en/function.stream-set-blocking.php
