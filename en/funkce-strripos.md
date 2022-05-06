PHP function strripos()
=======================

> id: '99f63413-4594-4a2d-8cc2-dc78e5622534'
> slug:
> 	cs: funkce-strripos
> 	en: php-function-strripos
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '20868d2756b242a5f509ca9397319dc9'

Availability in `PHP 5.0`

Find position of last occurrence of a case-insensitive string in a string


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$haystack` | `string` | *not* | The string to search in |
| `$needle` | `string` | *not* | Note that the needle may be a string of one or more characters. |
| `$offset` | `int` | null | The offset parameter may be specified to begin searching an arbitrary number of characters into the string. |


Return values
----------------

`int`

the numerical position of the last occurrence of
needle. Also note that string positions start at 0,
and not 1.
</p>
<p>
If needle is not found, false is returned.

Other resources
------------

[Official strripos documentation](https://www.php.net/manual/en/function.strripos.php)
