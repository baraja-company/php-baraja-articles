PHP function html_entity_decode()
=================================

> id: '54ac4951-8085-4cd8-9f75-114188fb6248'
> slug:
> 	cs: funkce-html-entity-decode
> 	en: php-function-html-entity-decode
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '7d96910f8c63e74b035c34f360fe76cb'

Availability in versions: `PHP 4.3.0`

Convert all HTML entities to their applicable characters


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$string` | `string` | *not* | String being processed. |
| `$quote_style` | `int` | null, | The optional second quote_style parameter lets you define what will be done with 'single' and "double" quotes. It takes on one of three constants with the default being ENT_COMPAT: <table> Available quote_style constants <tr valign="top"> <td>Constant Name</td> <td>Description</td> </tr> <tr valign="top"> <td>ENT_COMPAT</td> <td>Will convert double-quotes and leave single-quotes alone.</td> </tr> <tr valign="top"> <td>ENT_QUOTES</td> <td>Will convert both double and single quotes.</td> </tr> <tr valign="top"> <td>ENT_NOQUOTES</td> <td>Will leave both double and single quotes unconverted.</td> </tr> </table> |
| `$charset` | `string` | null | The ISO-8859-1 character set is used as default for the optional third charset. This defines the character set used in conversion. |


Return values
----------------

`string`

the decoded string.

Other resources
------------

[Official html-entity-decode documentation](https://www.php.net/manual/en/function.html-entity-decode.php)
