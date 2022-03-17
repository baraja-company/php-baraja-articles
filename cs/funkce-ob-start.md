PHP funkce ob_start()
=====================

> id: b2d74b3d-6ed4-453f-af14-d00c23d40c88
> slug:
> 	cs: funkce-ob-start
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Turn on output buffering


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$output_callback` | `callback` | null, | An optional output_callback function may be specified. This function takes a string as a parameter and should return a string. The function will be called when the output buffer is flushed (sent) or cleaned (with ob_flush, ob_clean or similar function) or when the output buffer is flushed to the browser at the end of the request. When output_callback is called, it will receive the contents of the output buffer as its parameter and is expected to return a new output buffer as a result, which will be sent to the browser. If the output_callback is not a callable function, this function will return false. |
| `$chunk_size` | `int` | null, | If the optional parameter chunk_size is passed, the buffer will be flushed after any output call which causes the buffer's length to equal or exceed chunk_size. Default value 0 means that the function is called only in the end, other special value 1 sets chunk_size to 4096. |
| `$erase` | `bool` | null | If the optional parameter erase is set to false, the buffer will not be deleted until the script finishes. This causes that flushing and cleaning functions would issue a notice and return false if called. |


Návratové hodnoty
----------------

`bool`

true on success or false on failure.

Další zdroje
------------

[Oficiální dokumentace funkce ob-start](https://www.php.net/manual/en/function.ob-start.php)
