PHP funkce stream_copy_to_stream()
==================================

> id: "2a32e374-fa9c-4c8a-93dc-3b5eb907cdb5"
> slug:
> 	cs: funkce-stream-copy-to-stream
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.0`

Copies data from one stream to another


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$source` | `resource` | *není* | The source stream |
| `$dest` | `resource` | *není* | The destination stream |
| `$maxlength` | `int` | null, | Maximum bytes to copy |
| `$offset` | `int` | null | The offset where to start to copy data |


Návratové hodnoty
----------------

`int`

the total count of bytes copied.

Další zdroje
------------

[Oficiální dokumentace funkce stream-copy-to-stream](https://www.php.net/manual/en/function.stream-copy-to-stream.php)
