PHP funkce stream_set_read_buffer()
===================================

> id: "6af12e27-142a-4a18-8d9f-9fc52bb38f70"
> slug:
> 	cs: funkce-stream-set-read-buffer
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.3.0`

Sets file buffering on the given stream


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$stream` | `resource` | *není* | The file pointer. |
| `$buffer` | `int` | *není* | The number of bytes to buffer. If buffer is 0 then write operations are unbuffered. This ensures that all writes with fwrite are completed before other processes are allowed to write to that output stream. |


Návratové hodnoty
----------------

`int`

0 on success, or EOF if the request cannot be honored.

Další zdroje
------------

[Oficiální dokumentace funkce stream-set-read-buffer](https://www.php.net/manual/en/function.stream-set-read-buffer.php)
