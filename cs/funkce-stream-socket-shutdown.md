PHP funkce stream_socket_shutdown()
===================================

> id: "0c9f4815-0984-4020-a584-728a5ac93a84"
> slug:
> 	cs: funkce-stream-socket-shutdown
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.2.1`

Shutdown a full-duplex connection


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$stream` | `resource` |  | An open stream (opened with stream_socket_client, for example) |
| `$how` | `int` |  | One of the following constants: STREAM_SHUT_RD (disable further receptions), STREAM_SHUT_WR (disable further transmissions) or STREAM_SHUT_RDWR (disable further receptions and transmissions). |


Návratové hodnoty
----------------

`bool`

true on success or false on failure.

Další zdroje
------------

https://www.php.net/manual/en/function.stream-socket-shutdown.php
