PHP function str_ireplace()
===========================

> id: e345edcc-0c33-4cf0-b99d-e14898a4e564
> slug:
> 	cs: funkce-str-ireplace
> 	en: php-function-str-ireplace
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: a899a50e5068cc95c5a5cbb2bef534ba

Availability in `PHP 5.0`

Case-insensitive version of <function>str_replace</function>.


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$search` | `mixed` | *not* | Every replacement with search array is performed on the result of previous replacement. |
| `$replace` | `mixed` | *not* | |
| `$subject` | `mixed` | *not* | If subject is an array, then the search and replace is performed with every entry of subject, and the return value is an array as well. |
| `$count` | `int` | null | The number of matched and replaced needles will be returned in count which is passed by reference. |


Return values
----------------

`mixed`

a string or an array of replacements.

Other resources
------------

[Official str-ireplace documentation](https://www.php.net/manual/en/function.str-ireplace.php)
