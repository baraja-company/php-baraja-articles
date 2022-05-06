PHP function strstr()
=====================

> id: '817c2566-9e01-4b1f-b36a-8a04a8ec5dae'
> slug:
> 	cs: funkce-strstr
> 	en: php-function-strstr
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '5fe03e35237306f52f6e1cb7eee0b84d'

Availability in `PHP 4.0`

Find first occurrence of a string


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$haystack` | `string` | *not* | String being processed. |
| `$needle` | `mixed` | *not* | If needle is not a string, it is converted to an integer and applied as the ordinal value of a character. |
| `$before_needle` | `bool` | null | If true, strstr returns the part of the haystack before the first occurrence of the needle. |


Return values
----------------

`string`

the portion of string, or false if needle
is not found.

Other resources
------------

[Official strstr documentation](https://www.php.net/manual/en/function.strstr.php)
