PHP funkce stream_socket_client()
=================================

> id: "8cc53341-41bc-4f68-ab07-ac46576f9bfc"
> slug:
> 	cs: funkce-stream-socket-client
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.0`

Open Internet or Unix domain socket connection


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$remote_socket` | `string` |  | Address to the socket to connect to. |
| `$errno` | `int` | null, | Will be set to the system level error number if connection fails. |
| `$errstr` | `string` | null, | Will be set to the system level error message if the connection fails. |
| `$timeout` | `float` | null, | Number of seconds until the connect() system call should timeout. This parameter only applies when not making asynchronous connection attempts. <p> To set a timeout for reading/writing data over the socket, use the stream_set_timeout, as the timeout only applies while making connecting the socket. |
| `$flags` | `int` | null, | Bitmask field which may be set to any combination of connection flags. Currently the select of connection flags is limited to STREAM_CLIENT_CONNECT (default), STREAM_CLIENT_ASYNC_CONNECT and STREAM_CLIENT_PERSISTENT. |
| `$context` | `resource` | null | A valid context resource created with stream_context_create. |


Návratové hodnoty
----------------

`resource`

|bool On success a stream resource is returned which may
be used together with the other file functions (such as
fgets, fgetss,
fwrite, fclose, and
feof), false on failure.

Další zdroje
------------

https://php.net/manual/en/function.stream-socket-client.php
