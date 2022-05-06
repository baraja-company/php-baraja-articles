PHP function stream_set_chunk_size()
====================================

> id: '49cfe6ae-10c7-4b3f-ad08-b6877ee6bc26'
> slug:
> 	cs: funkce-stream-set-chunk-size
> 	en: php-function-stream-set-chunk-size
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: b8f4c20ec2b551feaa78aa73987ca5e3

PHP > 5.4.0<br/>
Set the stream chunk size.


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$fp` | `resource` | *not* | |
| `$chunk_size` | `int` | *not* | |


Return values
----------------

`int`

Returns the previous chunk size on success.<br>
Will return <b>FALSE</b> if chunk_size is less than 1 or greater than <b>PHP_INT_MAX</b>.

Additional resources
------------

[Official documentation for stream-set-chunk-size](https://www.php.net/manual/en/function.stream-set-chunk-size.php)
