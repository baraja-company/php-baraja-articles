PHP function end()
==================

> id: '30dd1be2-0a2f-412e-ac86-a5aa05751391'
> slug:
> 	cs: funkce-end
> 	en: php-function-end
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '4abc7ce68ebe42ed0a6880f247e71497'

Availability in `PHP 4.0`

Sets an internal pointer to the last element of the array, which it also returns.

Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$array` | `array` | *not* | The array. This array is passed by reference because it is modified by the function. This means you must pass it a real variable and not a function returning an array because only actual variables may be passed by reference. |


Return values
----------------

`mixed` or `false`

Returns the last element of the array, or `false` if it is empty.

Other resources
------------

[Official end function documentation](https://www.php.net/manual/en/function.end.php)
