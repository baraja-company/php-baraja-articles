PHP function acos()
===================

> id: '94dfdeaa-3a05-43ce-aad3-e973c9cc4000'
> slug:
> 	cs: funkce-acos
> 	en: php-function-acos
> 
> publicationDate: '2020-02-16 16:41:10'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: e01af24917fcc55e21f0b15c57c14a31

Availability in `PHP 4.0`

Arcus cosinus (arcus cosinus).

The function returns the arcus cosine of the parameter `$arg` in radians. The `acos()` function is the inverse of the `cos()` function, which means that `$a === cos(acos($a))` (note the rounding!) for every value in the definition domain of the `acos()` function.

Parameters
---------

| Parameter | Data type | Default value | Note |
|----------|------------|-----|-----|
| `$arg` | `float` | *not* | Argument to process. |


Return values
----------------

`float`

Returns the arc cosine of the argument in radians.

Additional resources
------------

- <a href="/function-acosh">Function `acosh()`</a>
- [Official acos() documentation](https://www.php.net/manual/en/function.acos.php)
- <a href="/mathematics">How math works in PHP</a>
