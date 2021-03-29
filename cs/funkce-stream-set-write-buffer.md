PHP funkce stream_set_write_buffer()
====================================

> id: f59b327a-3bb0-40bc-b9fb-8a8d1c5dcda0
> slugCS: funkce-stream-set-write-buffer
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.3.0`

Sets file buffering on the given stream


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$stream` | `resource` |  | The file pointer. |
| `$buffer` | `int` |  | The number of bytes to buffer. If buffer is 0 then write operations are unbuffered. This ensures that all writes with fwrite are completed before other processes are allowed to write to that output stream. |


Návratové hodnoty
----------------

`int`

0 on success, or EOF if the request cannot be honored.

Další zdroje
------------

https://php.net/manual/en/function.stream-set-write-buffer.php
