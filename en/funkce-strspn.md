PHP function strspn()
=====================

> id: '03d42f61-2887-452b-9be3-2ddda936c17d'
> slug:
> 	cs: funkce-strspn
> 	en: php-function-strspn
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '87e738c87c02e63218d1e7f16ed0e8d6'

Availability in `PHP 4.0`

Finds the length of the first segment of a string consisting


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$subject` | `string` | *not* | The string to examine. |
| `$mask` | `string` | *not* | The list of allowable characters to include in counted segments. |
| `$start` | `int` | null, | The position in subject to start searching. |
| `$length` | `int` | null | The length of the segment from subject to examine. |


Return values
----------------

`int`

the length of the initial segment of str1
which consists entirely of characters in str2.

Other resources
------------

[Official strspn documentation](https://www.php.net/manual/en/function.strspn.php)
