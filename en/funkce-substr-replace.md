PHP function substr_replace()
=============================

> id: '63d688c9-bdd6-43ed-a1e8-44815c4a9f8e'
> slug:
> 	cs: funkce-substr-replace
> 	en: php-function-substr-replace
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '3283972d63dd56d1ffe131d646d2af28'

Availability in `PHP 4.0`

Replacing text in part of a string


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$string` | `mixed` | *not* | String being processed. |
| `$replacement` | `string` | *not* | Replacement string. |
 | `$start` | `int` | *not* | If start is positive, the replacement will start from the initial offset into the string. |
 | `$length` | `int` | null | If specified and positive, represents the length of the part of the string to be replaced. If negative, it represents the number of characters from the end of the string at which the replacement is to stop. If not specified, then the value strlen( string ) is used by default; that is, the replacement is terminated at the end of the string. Of course, if the length is zero, then this function will result in the substitution being inserted into the string at the specified starting offset. |


Return values
----------------

`mixed`

Returns the resulting string. If the string is an array, an array is returned.

Additional resources
------------

[Official substr-replace documentation](https://www.php.net/manual/en/function.substr-replace.php)
