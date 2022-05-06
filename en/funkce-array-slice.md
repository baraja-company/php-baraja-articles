PHP function array_slice()
==========================

> id: df678ab7-3929-4833-9223-0afdca5f94d9
> slug:
> 	cs: funkce-array-slice
> 	en: php-function-array-slice
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '59874540-664b-4474-9869-7e6742ab6051'
> sourceContentHash: b6e4b4869397c8b170a78e3c0493f205

Availability in `PHP 4.0`

Extract a slice of the array


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$array` | `array` | *not* | The input array. |
| `$offset` | `int` | *not* | If offset is non-negative, the sequence will start at that offset in the array. If offset is negative, the sequence will start that far from the end of the array. |
| `$length` | `int` | null, | If length is given and is positive, then the sequence will have that many elements in it. If length is given and is negative then the sequence will stop that many elements from the end of the array. If it is omitted, then the sequence will have everything from offset up until the end of the array. |
| `$preserve_keys` | `bool` | false | Note that array_slice will reorder and reset the array indices by default. You can change this behavior by setting preserve_keys to true. |


Return values
----------------

`array`

the slice.

Other resources
------------

[Official array-slice documentation](https://www.php.net/manual/en/function.array-slice.php)
