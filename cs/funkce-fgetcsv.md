PHP funkce fgetcsv()
====================

> id: "84a4c227-67ec-457b-87a4-09f07828cb5c"
> slug:
> 	cs: funkce-fgetcsv
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Gets line from file pointer and parse for CSV fields


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$handle` | `resource` | *není* | A valid file pointer to a file successfully opened by fopen, popen, or fsockopen. |
| `$length` | `int` | null, | Must be greater than the longest line (in characters) to be found in the CSV file (allowing for trailing line-end characters). It became optional in PHP 5. Omitting this parameter (or setting it to 0 in PHP 5.0.4 and later) the maximum line length is not limited, which is slightly slower. |
| `$delimiter` | `string` | null, | Set the field delimiter (one character only). |
| `$enclosure` | `string` | null, | Set the field enclosure character (one character only). |
| `$escape` | `string` | null | Set the escape character (one character only). Defaults as a backslash. |


Návratové hodnoty
----------------

`array`

an indexed array containing the fields read.
</p>
<p>
A blank line in a CSV file will be returned as an array
comprising a single null field, and will not be treated
as an error.
</p>
&note.line-endings;
<p>
fgetcsv returns &null; if an invalid
handle is supplied or false on other errors,
including end of file.

Další zdroje
------------

[Oficiální dokumentace funkce fgetcsv](https://www.php.net/manual/en/function.fgetcsv.php)
