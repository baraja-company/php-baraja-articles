PHP function stream_socket_server()
===================================

> id: c86ca4ad-975f-4a8c-ac5b-6f59e5d10d0b
> slug:
> 	cs: funkce-stream-socket-server
> 	en: php-function-stream-socket-server
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '5348bbe2efbce80d25e29e9457dee244'

Availability in `PHP 5.0`

Create an Internet or Unix domain server socket


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$local_socket` | `string` | *not* | The type of socket created is determined by the transport specified using standard URL formatting: transport://target. |
| `$errno` | `int` | null, | If the optional errno and errstr arguments are present they will be set to indicate the actual system-level error that occurred in the system-level socket(), bind(), and listen() calls. If the value returned in errno is 0 and the function returned false, it is an indication that the error occurred before the bind() call. This is most likely due to a problem initializing the socket. Note that the errno and errstr arguments will always be passed by reference. |
| `$errstr` | `string` | null, | See errno description. |
| `$flags` | `int` | null, | A bitmask field which may be set to any combination of socket creation flags. |
| | `$context` | `resource` | null |


Return values
----------------

`resource`

the created stream, or false on error.

Other resources
------------

[Official stream-socket-server documentation](https://www.php.net/manual/en/function.stream-socket-server.php)
