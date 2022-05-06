PHP function stream_socket_accept()
===================================

> id: de5a0476-2f97-4962-9f94-a40f13c12327
> slug:
> 	cs: funkce-stream-socket-accept
> 	en: php-function-stream-socket-accept
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '11a4acc57638fc65429bfd5891bcb0ce'

Availability in `PHP 5.0`

Accept a connection on a socket created by <function>stream_socket_server</function>


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$server_socket` | `resource` | *not* | Override the default socket accept timeout. Time should be given in seconds. |
| `$timeout` | `float` | null, | Override the default socket accept timeout. Time should be given in seconds. |
| `$peername` | `string` | null | Will be set to the name (address) of the client which connected, if included and available from the selected transport. |


Return values
----------------

`resource`

|bool Returns a stream to the accepted socket connection or FALSE on failure.

Other resources
------------

[Official documentation for stream-socket-accept](https://www.php.net/manual/en/function.stream-socket-accept.php)
