PHP function stream_set_read_buffer()
=====================================

> id: '6af12e27-142a-4a18-8d9f-9fc52bb38f70'
> slug:
> 	cs: funkce-stream-set-read-buffer
> 	en: php-function-stream-set-read-buffer
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '92782d6c257e7f76c4f09e4f40797022'

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

[Official stream-set-read-buffer documentation](https://www.php.net/manual/en/function.stream-set-read-buffer.php)
