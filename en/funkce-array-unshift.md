PHP function array_unshift()
============================

> id: '1933c5e2-c775-47ca-8887-48da5c101371'
> slug:
> 	cs: funkce-array-unshift
> 	en: php-function-array-unshift
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '59874540-664b-4474-9869-7e6742ab6051'
> sourceContentHash: '0e7ae9b4c7d88fb52f2302738cd6dc0d'

Availability in `PHP 4.0`

This function appends one or more elements to the beginning of an array.

> **Warning:** Numeric indices will be renumbered from zero. The function resets the internal pointer to the current element in the array.

Using
--------

```php
array_unshift(array array, mixed var [, mixed ...])
```

This function takes the passed element and places it at the beginning of a new array. It then copies the original array after the passed element, preserving the keys and order.

The function returns the new number of elements in the array. However, the array is modified via reference, so it needs to be read from the variable in which it was passed.

An example usage is:

```php
$queue = ['p1', 'p3'];
array_unshift($queue, 'p4', 'p5', 'p6');
```

The modified array `$queue` will have 5 elements: 'p4', 'p5', 'p6', 'p1' and 'p3'.

Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$array` | `array` | *not* | Modified array. |
| `$var` | `mixed` | *not* | The value that goes to the beginning of the array. |
| `$_` | `mixed` | null | Optional additional values that will go to the top of the array. |

Return values
----------------

Returns `int` representing the new number of elements in the array.

Additional resources
------------

[Official documentation for the array-unshift function]([Official documentation](https://www.php.net/manual/en/function.array-unshift.php))
