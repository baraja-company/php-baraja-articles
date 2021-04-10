PHP funkce htmlspecialchars_decode()
====================================

> id: c2fef686-4c02-4698-9963-49d76435cf7f
> slug:
> 	cs: funkce-htmlspecialchars-decode
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.1.0`

Convert special HTML entities back to characters


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$string` | `string` |  | The string to decode |
| `$quote_style` | `int` | null | The quote style. One of the following constants: <table> quote_style constants <tr valign="top"> <td>Constant Name</td> <td>Description</td> </tr> <tr valign="top"> <td>ENT_COMPAT</td> <td>Will convert double-quotes and leave single-quotes alone (default)</td> </tr> <tr valign="top"> <td>ENT_QUOTES</td> <td>Will convert both double and single quotes</td> </tr> <tr valign="top"> <td>ENT_NOQUOTES</td> <td>Will leave both double and single quotes unconverted</td> </tr> </table> |


Návratové hodnoty
----------------

`string`

the decoded string.

Další zdroje
------------

https://php.net/manual/en/function.htmlspecialchars-decode.php
