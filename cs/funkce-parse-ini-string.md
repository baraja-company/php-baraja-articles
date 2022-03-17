PHP funkce parse_ini_string()
=============================

> id: "82a91c92-38e8-4a37-a4b4-26f52755f240"
> slug:
> 	cs: funkce-parse-ini-string
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.3.0`

Parse a configuration string


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$ini` | `string` | *není* | The contents of the ini file being parsed. |
| `$process_sections` | `bool` | null, | By setting the process_sections parameter to true, you get a multidimensional array, with the section names and settings included. The default for process_sections is false |
| `$scanner_mode` | `int` | null | Can either be INI_SCANNER_NORMAL (default) or INI_SCANNER_RAW. If INI_SCANNER_RAW is supplied, then option values will not be parsed. |


Návratové hodnoty
----------------

`array`

|bool The settings are returned as an associative array on success,
and false on failure.

Další zdroje
------------

[Oficiální dokumentace funkce parse-ini-string](https://www.php.net/manual/en/function.parse-ini-string.php)
