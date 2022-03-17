PHP funkce socket_set_blocking()
================================

> id: "1e938780-cb86-4aa2-9893-26167adde9cf"
> slug:
> 	cs: funkce-socket-set-blocking
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

&Alias; <function>stream_set_blocking</function>


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$socket` | `resource` | *není* | The stream. |
| `$mode` | `int` | *není* | If mode is 0, the given stream will be switched to non-blocking mode, and if 1, it will be switched to blocking mode. This affects calls like fgets and fread that read from the stream. In non-blocking mode an fgets call will always return right away while in blocking mode it will wait for data to become available on the stream. |


Návratové hodnoty
----------------

`bool`

true on success or false on failure.

Další zdroje
------------

[Oficiální dokumentace funkce socket-set-blocking](https://www.php.net/manual/en/function.socket-set-blocking.php)
