PHP function stream_socket_shutdown()
=====================================

> id: '0c9f4815-0984-4020-a584-728a5ac93a84'
> slug:
> 	cs: funkce-stream-socket-shutdown
> 	en: php-function-stream-socket-shutdown
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '3604ece727264a23b619e0b7d09e195e'

Availability in versions: `PHP 5.2.1`

Shutdown a full-duplex connection


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$stream` | `resource` | *not* | An open stream (opened with stream_socket_client, for example) |
| `$how` | `int` | *not* | One of the following constants: STREAM_SHUT_RD (disable further receptions), STREAM_SHUT_WR (disable further transmissions) or STREAM_SHUT_RDWR (disable further receptions and transmissions). |


Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.

Other resources
------------

[Official documentation for stream-socket-shutdown](https://www.php.net/manual/en/function.stream-socket-shutdown.php)
