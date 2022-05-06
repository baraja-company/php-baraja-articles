PHP function htmlentities()
===========================

> id: '6317d03b-4990-4ba5-979e-cdaf583c589a'
> slug:
> 	cs: funkce-htmlentities
> 	en: php-function-htmlentities
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: a7e41e5237939ae7c86735ce3a020db6

Availability in `PHP 4.0`

Convert all applicable characters to HTML entities


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$string` | `string` | *not* | String being processed. |
| `$quote_style` | `int` | null, | Like htmlspecialchars, the optional second quote_style parameter lets you define what will be done with 'single' and "double" quotes. It takes on one of three constants with the default being ENT_COMPAT: <table> Available quote_style constants <tr valign="top"> <td>Constant Name</td> <td>Description</td> </tr> <tr valign="top"> <td>ENT_COMPAT</td> <td>Will convert double-quotes and leave single-quotes alone.</td> </tr> <tr valign="top"> <td>ENT_QUOTES</td> <td>Will convert both double and single quotes.</td> </tr> <tr valign="top"> <td>ENT_NOQUOTES</td> <td>Will leave both double and single quotes unconverted.</td> </tr> </table> |
| `$charset` | `string` | null, | Like htmlspecialchars, it takes an optional third argument charset which defines the character set used in conversion. Presently, the ISO-8859-1 character set is used as the default. |
| `$double_encode` | `bool` | true | When double_encode is turned off PHP will not encode existing html entities. The default is to convert everything. |


Return values
----------------

`string`

the encoded string.

Other resources
------------

[Official htmlentities documentation](https://www.php.net/manual/en/function.htmlentities.php)
