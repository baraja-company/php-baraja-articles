PHP funkce highlight_file()
===========================

> id: "4f004290-2dfa-46a4-8075-4fd318714de4"
> slug:
> 	cs: funkce-highlight-file
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Syntax highlighting of a file


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` |  | Path to the PHP file to be highlighted. |
| `$return` | `bool` | null | Set this parameter to true to make this function return the highlighted code. |


Návratové hodnoty
----------------

`string`

|bool If return is set to true, returns the highlighted
code as a string instead of printing it out. Otherwise, it will return
true on success, false on failure.

Další zdroje
------------

https://php.net/manual/en/function.highlight-file.php
