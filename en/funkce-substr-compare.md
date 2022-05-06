PHP function substr_compare()
=============================

> id: '67b0a6e5-9779-4326-a489-53e114b777a5'
> slug:
> 	cs: funkce-substr-compare
> 	en: php-function-substr-compare
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '07edd1e92d3430a4f9e2c755c9da19ad'

Availability in `PHP 5.0`

Binary safe comparison of 2 strings from offset up to character length


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$main_str` | `string` | *not* | Main string being compared. |
| `$str` | `string` | *not* | Main string being compared. |
| `$offset` | `int` | *not* | Starting position for comparison. If negative, it starts counting from the end of the string. |
| `$length` | `int` | null, | Length of the comparison. |
| `$case_insensitivity` | `bool` | null | If case_insensitivity is true, the comparison is not case sensitive. |


Return values
----------------

`int`

- `0` if `main_str` of offset position is less than str
- `0` if it is greater than str
- `0` if they are equal.

If offset is equal to or greater than the length of `main_str` or the length is set and is less than `1`, substr_compare prints a warning and returns false.

Additional resources
------------

[Official substr-compare documentation](https://www.php.net/manual/en/function.substr-compare.php)
