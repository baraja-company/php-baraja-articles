PHP function set_socket_blocking()
==================================

> id: '1fce4f69-4542-4f7e-ad2c-5e1fadf65f74'
> slug:
> 	cs: funkce-set-socket-blocking
> 	en: php-function-set-socket-blocking
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: dddbb76a15c4d86022d9ff1aa9a43191

Availability in `PHP 4.0`

&Alias; <function>stream_set_blocking</function>
<p>Sets blocking or non-blocking mode on a stream.
This function works for any stream that supports non-blocking mode (currently, regular files and socket streams).


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$socket` | `resource` | *not* | |
| `$mode` | `int` | *not* | |


Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.
