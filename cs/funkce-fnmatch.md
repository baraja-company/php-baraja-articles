PHP funkce fnmatch()
================================

> id: 80c9e5c8-ce91-482f-93ea-94cc753c6804
> slugCS: funkce-fnmatch
> publicationDate: 2019-09-11 10:04:03
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 4.3.0`

Match filename against a pattern


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$pattern` | `string` |  | The shell wildcard pattern. |
| `$string` | `string` |  | The tested string. This function is especially useful for filenames, but may also be used on regular strings. |
| `$flags` | `int` | null | The value of flags can be any combination of the following flags, joined with the binary OR (|) operator. <table> A list of possible flags for fnmatch <tr valign="top"> <td>Flag</td> <td>Description</td> </tr> <tr valign="top"> <td>FNM_NOESCAPE</td> <td> Disable backslash escaping. </td> </tr> <tr valign="top"> <td>FNM_PATHNAME</td> <td> Slash in string only matches slash in the given pattern. </td> </tr> <tr valign="top"> <td>FNM_PERIOD</td> <td> Leading period in string must be exactly matched by period in the given pattern. </td> </tr> <tr valign="top"> <td>FNM_CASEFOLD</td> <td> Caseless match. Part of the GNU extension. </td> </tr> </table> |


Návratové hodnoty
----------------

`bool`

true if there is a match, false otherwise.

Další zdroje
------------

https://php.net/manual/en/function.fnmatch.php
