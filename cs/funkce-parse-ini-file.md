PHP funkce parse_ini_file()
===========================

> id: d58e96e6-670d-422b-8a0d-b255a9d66692
> slug:
> 	cs: funkce-parse-ini-file
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Parse a configuration file


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` | *není* | The filename of the ini file being parsed. |
| `$process_sections` | `bool` | null, | By setting the process_sections parameter to true, you get a multidimensional array, with the section names and settings included. The default for process_sections is false |
| `$scanner_mode` | `int` | null | Can either be INI_SCANNER_NORMAL (default) or INI_SCANNER_RAW. If INI_SCANNER_RAW is supplied, then option values will not be parsed. |


Návratové hodnoty
----------------

`array`

|bool The settings are returned as an associative array on success,
and false on failure.

Další zdroje
------------

https://www.php.net/manual/en/function.parse-ini-file.php
