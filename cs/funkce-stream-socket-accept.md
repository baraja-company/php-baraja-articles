PHP funkce stream_socket_accept()
================================

> id: de5a0476-2f97-4962-9f94-a40f13c12327
> slugCS: funkce-stream-socket-accept
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.0`

Accept a connection on a socket created by <function>stream_socket_server</function>


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$server_socket` | `resource` |  | Override the default socket accept timeout. Time should be given in seconds. |
| `$timeout` | `float` | null, | Override the default socket accept timeout. Time should be given in seconds. |
| `$peername` | `string` | null | Will be set to the name (address) of the client which connected, if included and available from the selected transport. |


Návratové hodnoty
----------------

`resource`

|bool Returns a stream to the accepted socket connection or FALSE on failure.

Další zdroje
------------

https://php.net/manual/en/function.stream-socket-accept.php
