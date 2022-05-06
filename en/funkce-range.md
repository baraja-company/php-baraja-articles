PHP function range()
====================

> id: b8a6d78a-2b24-40c2-96ac-fb1f999ad340
> slug:
> 	cs: funkce-range
> 	en: php-function-range
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '0716207f8ac9e88f10341dd5e8a519e2'

Availability in `PHP 4.0`

Create an array containing a range of elements


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$low` | `mixed` | *not* | Low value. |
| `$high` | `mixed` | *not* | High value. |
| `$step` | `number` | null | If a step value is given, it will be used as the increment between elements in the sequence. step should be given as a positive number. If not specified, step will default to 1. |


Return values
----------------

`array`

an array of elements from low to
high, inclusive. If low > high, the sequence will
be from high to low.

Other resources
------------

[Official documentation of the range function](https://www.php.net/manual/en/function.range.php)
