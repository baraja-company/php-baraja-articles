PHP function stream_socket_get_name()
=====================================

> id: b732a473-ed8b-468f-921e-e1016a9dc946
> slug:
> 	cs: funkce-stream-socket-get-name
> 	en: php-function-stream-socket-get-name
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: bf81e53ab5187c02ba12f1daf95e8726

Availability in `PHP 5.0`

Retrieve the name of the local or remote sockets


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$handle` | `resource` | *not* | The socket to get the name of. |
| `$want_peer` | `bool` | *not* | If set to true the remote socket name will be returned, if set to false the local socket name will be returned. |


Return values
----------------

`string`

The name of the socket.

Other resources
------------

[Official documentation for stream-socket-get-name](https://www.php.net/manual/en/function.stream-socket-get-name.php)
