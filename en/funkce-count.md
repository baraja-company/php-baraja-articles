PHP function count()
====================

> id: '5a07a47e-4e88-4d7c-8c1d-d6818aea0d8b'
> slug:
> 	cs: funkce-count
> 	en: php-function-count
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '978f97f1d1a247f1eb31dd102c5990cc'

Availability in `PHP 4.0`

Counts all elements in an array, or something in an object.
<p>For objects, if you have SPL installed, you can hook into count() by implementing interface {

Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$var` | `array|Countable` | *not* | |
| `$mode` | `int` | COUNT_NORMAL | |


Return values
----------------

`int`

the number of elements in var, which is
typically an array, since anything else will have one
element.
</p>
<p>
If var is not an array or an object with
implemented Countable interface,
1 will be returned.
There is one exception, if var is &null;,
0 will be returned.
</p>
<p>
Caution: count may return 0 for a variable that isn't set,
but it may also return 0 for a variable that has been initialized with an
empty array. Use isset to test if a variable is set.

Other resources
------------

[Official documentation for the count function](https://www.php.net/manual/en/function.count.php)
