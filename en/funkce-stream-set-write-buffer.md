PHP function stream_set_write_buffer()
======================================

> id: f59b327a-3bb0-40bc-b9fb-8a8d1c5dcda0
> slug:
> 	cs: funkce-stream-set-write-buffer
> 	en: php-function-stream-set-write-buffer
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: cf3482d2cbc59fa40403313aa4b4841b

Availability in versions: `PHP 4.3.0`

Sets file buffering on the given stream


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$stream` | `resource` | *not* | The file pointer. |
| `$buffer` | `int` | *not* | The number of bytes to buffer. If buffer is 0 then write operations are unbuffered. This ensures that all writes with fwrite are completed before other processes are allowed to write to that output stream. |


Return values
----------------

`int`

0 on success, or EOF if the request cannot be honored.

Other resources
------------

[Official stream-set-write-buffer documentation](https://www.php.net/manual/en/function.stream-set-write-buffer.php)
