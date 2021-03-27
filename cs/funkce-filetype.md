PHP funkce filetype()
================================

> id: c11a3af6-ef35-44de-b171-f22eb58ed6d1
> slugCS: funkce-filetype
> publicationDate: 2019-09-11 10:04:03
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 4.0`

Gets file type


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` |  | Path to the file. |


Návratové hodnoty
----------------

`string`

the type of the file. Possible values are fifo, char,
dir, block, link, file, socket and unknown.
</p>
<p>
Returns false if an error occurs. filetype will also
produce an E_NOTICE message if the stat call fails
or if the file type is unknown.

Další zdroje
------------

https://php.net/manual/en/function.filetype.php
