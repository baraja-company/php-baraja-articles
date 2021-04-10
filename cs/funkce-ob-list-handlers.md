PHP funkce ob_list_handlers()
=============================

> id: "7be89d03-94e9-4bb7-b454-2e0561eb073c"
> slug:
> 	cs: funkce-ob-list-handlers
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.3.0`

List all output handlers in use


Parametry
--------------

Funkce nemá žádné vstupní parametry.

Návratové hodnoty
----------------

`array`

This will return an array with the output handlers in use (if any). If
output_buffering is enabled or
an anonymous function was used with ob_start,
ob_list_handlers will return "default output
handler".

Další zdroje
------------

https://php.net/manual/en/function.ob-list-handlers.php
