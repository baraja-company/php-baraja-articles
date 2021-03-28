PHP funkce stream_get_line()
================================

> id: "164a0ff1-9be1-4ef0-8494-577d1e0b55a0"
> slugCS: funkce-stream-get-line
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.0`

Gets line from stream resource up to a given delimiter


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$handle` | `resource` |  | A valid file handle. |
| `$length` | `int` |  | The number of bytes to read from the handle. |
| `$ending` | `string` | null | An optional string delimiter. |


Návratové hodnoty
----------------

`string`

a string of up to length bytes read from the file
pointed to by handle.
</p>
<p>
If an error occurs, returns false.

Další zdroje
------------

https://php.net/manual/en/function.stream-get-line.php
