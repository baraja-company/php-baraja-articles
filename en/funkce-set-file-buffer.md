PHP function set_file_buffer()
==============================

> id: '4da411ce-7a76-418a-97cf-e4f221863b8b'
> slug:
> 	cs: funkce-set-file-buffer
> 	en: php-function-set-file-buffer
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: acbab0213f0436fbafbb0df13f34fc9c

Availability in `PHP 4.0`

&Alias; <function>stream_set_write_buffer</function>
<p>Sets the buffering for write operations on the given stream to buffer bytes.
Output using fwrite() is normally buffered at 8K.
This means that if there are two processes wanting to write to the same output stream (a file),
each is paused after 8K of data to allow the other to write.


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$fp` | `resource` | *not* | |
| `$buffer` | `int` | *not* | |


Return values
----------------

`int`



Other resources
------------

[Official set-file-buffer documentation](https://www.php.net/manual/en/function.set-file-buffer.php)
