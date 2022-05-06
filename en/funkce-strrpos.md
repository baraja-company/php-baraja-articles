PHP function strrpos()
======================

> id: c2d989c9-94bc-4f21-838a-b9465065d4a9
> slug:
> 	cs: funkce-strrpos
> 	en: php-function-strrpos
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: a0a027c0d9858f855a6bc85c7d1aa120

Availability in `PHP 4.0`

Find the position of the last occurrence of a substring in a string


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$haystack` | `string` | *not* | The string to search in. |
| `$needle` | `string` | *not* | If <b>needle</b> is not a string, it is converted to an integer and applied as the ordinal value of a character. |
| `$offset` | `int` | 0 | If specified, search will start this number of characters counted from the beginning of the string. If the value is negative, search will instead start from that many characters from the end of the string, searching backwards. |


Return values
----------------

`int`

|boolean <p>
Returns the position where the needle exists relative to the beginning of
the <b>haystack</b> string (independent of search direction
or offset).
Also note that string positions start at 0, and not 1.
</p>
<p>
Returns <b>FALSE</b> if the needle was not found.
</p>

Other resources
------------

[Official strrpos documentation](https://www.php.net/manual/en/function.strrpos.php)
