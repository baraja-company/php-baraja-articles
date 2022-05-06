PHP function socket_set_blocking()
==================================

> id: '1e938780-cb86-4aa2-9893-26167adde9cf'
> slug:
> 	cs: funkce-socket-set-blocking
> 	en: php-function-socket-set-blocking
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '4c0515dd25c4d3168a748e64e99fb07b'

Availability in `PHP 4.0`

&Alias; <function>stream_set_blocking</function>


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$socket` | `resource` | *not* | The stream. |
| `$mode` | `int` | *not* | If mode is 0, the given stream will be switched to non-blocking mode, and if 1, it will be switched to blocking mode. This affects calls like fgets and fread that read from the stream. In non-blocking mode an fgets call will always return right away while in blocking mode it will wait for data to become available on the stream. |


Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.

Other resources
------------

[Official socket-set-blocking documentation](https://www.php.net/manual/en/function.socket-set-blocking.php)
