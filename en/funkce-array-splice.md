PHP function array_splice()
===========================

> id: '733af217-e55b-4fa4-a1d1-19a7b21f3ce9'
> slug:
> 	cs: funkce-array-splice
> 	en: php-function-array-splice
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '59874540-664b-4474-9869-7e6742ab6051'
> sourceContentHash: '197bc5b8e0d2f6031a0bd0a5a1a86378'

Availability in `PHP 4.0`

Remove a portion of the array and replace it with something else


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$input` | `array` | *not* | The input array. |
| `$offset` | `int` | *not* | If offset is positive then the start of removed portion is at that offset from the beginning of the input array. If offset is negative then it starts that far from the end of the input array. |
| `$length` | `int` | null, | If length is omitted, removes everything from offset to the end of the array. If length is specified and is positive, then that many elements will be removed. If length is specified and is negative then the end of the removed portion will be that many elements from the end of the array. Tip: to remove everything from offset to the end of the array when replacement is also specified, use count($input) for length. |
| `$replacement` | `mixed` | null | If replacement array is specified, then the removed elements are replaced with elements from this array. |


Return values
----------------

`array`

the array consisting of the extracted elements.

Additional resources
------------

[Official documentation of the array-splice function](https://www.php.net/manual/en/function.array-splice.php)
