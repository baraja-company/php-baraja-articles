PHP funkce stream_set_chunk_size()
==================================

> id: "49cfe6ae-10c7-4b3f-ad08-b6877ee6bc26"
> slug:
> 	cs: funkce-stream-set-chunk-size
> 
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

PHP > 5.4.0<br/>
Set the stream chunk size.


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$fp` | `resource` |  |  |
| `$chunk_size` | `int` |  |  |


Návratové hodnoty
----------------

`int`

Returns the previous chunk size on success.<br>
Will return <b>FALSE</b> if chunk_size is less than 1 or greater than <b>PHP_INT_MAX</b>.

Další zdroje
------------

https://www.php.net/manual/en/function.stream-set-chunk-size.php
