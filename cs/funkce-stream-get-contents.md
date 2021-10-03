PHP funkce stream_get_contents()
================================

> id: "3fd19ad5-566c-488b-bd67-e13be297c480"
> slug:
> 	cs: funkce-stream-get-contents
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.0`

Reads remainder of a stream into a string


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$handle` | `resource` |  | A stream resource (e.g. returned from fopen) |
| `$maxlength` | `int` | null, | The maximum bytes to read. Defaults to -1 (read all the remaining buffer). |
| `$offset` | `int` | null | Seek to the specified offset before reading. |


Návratové hodnoty
----------------

`string`

|bool a string or false on failure.

Další zdroje
------------

https://www.php.net/manual/en/function.stream-get-contents.php
