PHP function money_format()
===========================

> id: eb7649d0-1509-45f6-a35a-33812fe68758
> slug:
> 	cs: funkce-money-format
> 	en: php-function-money-format
> 
> publicationDate: '2019-09-11 10:04:00'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: a9c2b90d6ca73d4880d9e6ef851a8f97

Availability in versions: `PHP 4.3.0`

Formats a number as a currency string


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$format` | `string` | *not* | The format specification consists of the following sequence: <p>a % character |
| `$number` | `float` | *not* | The number to be formatted. |


Return values
----------------

`string`

the formatted string. Characters before and after the formatting
string will be returned unchanged.
Non-numeric number causes returning &null; and
emitting E_WARNING.

Other resources
------------

[Official money-format documentation](https://www.php.net/manual/en/function.money-format.php)
