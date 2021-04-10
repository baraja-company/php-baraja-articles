PHP funkce headers_sent()
=========================

> id: "3cf42d2b-7dcd-418d-a5d6-d3674a8aa23c"
> slug:
> 	cs: funkce-headers-sent
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Checks if or where headers have been sent


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$file` | `string` | null, | If the optional file and line parameters are set, headers_sent will put the PHP source file name and line number where output started in the file and line variables. |
| `$line` | `int` | null | The line number where the output started. |


Návratové hodnoty
----------------

`bool`

headers_sent will return false if no HTTP headers
have already been sent or true otherwise.

Další zdroje
------------

https://php.net/manual/en/function.headers-sent.php
