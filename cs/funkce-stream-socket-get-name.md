PHP funkce stream_socket_get_name()
===================================

> id: b732a473-ed8b-468f-921e-e1016a9dc946
> slugCS: funkce-stream-socket-get-name
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.0`

Retrieve the name of the local or remote sockets


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$handle` | `resource` |  | The socket to get the name of. |
| `$want_peer` | `bool` |  | If set to true the remote socket name will be returned, if set to false the local socket name will be returned. |


Návratové hodnoty
----------------

`string`

The name of the socket.

Další zdroje
------------

https://php.net/manual/en/function.stream-socket-get-name.php
