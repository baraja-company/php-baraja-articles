PHP funkce fgetss()
===================

> id: "39d98959-d30c-4757-8122-7768e5895484"
> slug:
> 	cs: funkce-fgetss
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Gets line from file pointer and strip HTML tags


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$handle` | `resource` |  |  |
| `$length` | `int` | null, | Length of the data to be retrieved. |
| `$allowable_tags` | `string` | null | You can use the optional third parameter to specify tags which should not be stripped. |


Návratové hodnoty
----------------

`string`

a string of up to length - 1 bytes read from
the file pointed to by handle, with all HTML and PHP
code stripped.
</p>
<p>
If an error occurs, returns false.

Další zdroje
------------

https://www.php.net/manual/en/function.fgetss.php
