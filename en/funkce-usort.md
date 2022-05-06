PHP function usort()
====================

> id: '34e70aad-f369-4d43-87fb-9f3b7287c97e'
> slug:
> 	cs: funkce-usort
> 	en: php-function-usort
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: c1b7dfa375f0e9ff03efb52082fb213f

Availability in `PHP 4.0`

Sort an array by values using a user-defined comparison function


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$array` | `array` | *not* | The input array. |
| `$cmp_function` | `callback` | *not* | The comparison function must return an integer less than, equal to, or greater than zero if the first argument is considered to be respectively less than, equal to, or greater than the second. |


Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.

Other resources
------------

[Official usort documentation](https://www.php.net/manual/en/function.usort.php)
