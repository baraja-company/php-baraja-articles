PHP funkce fgets()
==================

> id: e8e2483e-02fc-410b-8e02-f46d73069db0
> slug:
> 	cs: funkce-fgets
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Gets line from file pointer


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$handle` | `resource` |  |  |
| `$length` | `int` | null | Reading ends when length - 1 bytes have been read, on a newline (which is included in the return value), or on EOF (whichever comes first). If no length is specified, it will keep reading from the stream until it reaches the end of the line. |


Návratové hodnoty
----------------

`string`

a string of up to length - 1 bytes read from
the file pointed to by handle.
</p>
<p>
If an error occurs, returns false.

Další zdroje
------------

https://php.net/manual/en/function.fgets.php
