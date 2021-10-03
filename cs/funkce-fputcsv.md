PHP funkce fputcsv()
====================

> id: c718ddd4-d5ce-4c3e-bbd8-2f4989d19b00
> slug:
> 	cs: funkce-fputcsv
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.1.0`

Format line as CSV and write to file pointer


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$handle` | `resource` |  |  |
| `$fields` | `array` |  | An array of values. |
| `$delimiter` | `string` | ",", | The optional delimiter parameter sets the field delimiter (one character only). |
| `$enclosure` | `string` | '"', | The optional enclosure parameter sets the field enclosure (one character only). |
| `$escape_char` | `string` | "\\" |  |


Návratové hodnoty
----------------

`int`

|bool the length of the written string or false on failure.

Další zdroje
------------

https://www.php.net/manual/en/function.fputcsv.php
