PHP function stream_get_line()
==============================

> id: '164a0ff1-9be1-4ef0-8494-577d1e0b55a0'
> slug:
> 	cs: funkce-stream-get-line
> 	en: php-function-stream-get-line
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '07684c5246eed879e42cda40fee77932'

Availability in `PHP 5.0`

Gets line from stream resource up to a given delimiter


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$handle` | `resource` | *not* | A valid file handle. |
| `$length` | `int` | *not* | The number of bytes to read from the handle. |
| `$ending` | `string` | null | An optional string delimiter. |


Return values
----------------

`string`

a string of up to length bytes read from the file
pointed to by handle.
</p>
<p>
If an error occurs, returns false.

Other resources
------------

[Official stream-get-line documentation](https://www.php.net/manual/en/function.stream-get-line.php)
