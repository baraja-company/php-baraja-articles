PHP function count_chars()
==========================

> id: e48cf8d3-a048-4e33-961e-aa6a04e122e7
> slug:
> 	cs: funkce-count-chars
> 	en: php-function-count-chars
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '9ef8d743c7f3adf3588eab0241e8f938'

Availability in `PHP 4.0`

Return information about characters used in a string


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$string` | `string` | *not* | The examined string. |
| `$mode` | `int` | null | See return values. |


Return values
----------------

`mixed`

Depending on mode
count_chars returns one of the following:
0 - an array with the byte-value as key and the frequency of
every byte as value.
1 - same as 0 but only byte-values with a frequency greater
than zero are listed.
2 - same as 0 but only byte-values with a frequency equal to
zero are listed.
3 - a string containing all unique characters is returned.
4 - a string containing all not used characters is returned.

Other resources
------------

[Official documentation of the count-chars function](https://www.php.net/manual/en/function.count-chars.php)
