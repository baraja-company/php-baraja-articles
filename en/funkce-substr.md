PHP function substr()
=====================

> id: f74977bc-9004-4210-84a1-b8d0e8352795
> slug:
> 	cs: funkce-substr
> 	en: php-function-substr
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '34b55900e4ea07986495879d73fd5070'

Availability in `PHP 4.0`

Return part of a string


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$string` | `string` | *not* | String being processed. |
| `$start` | `int` | *not* | If start is non-negative, the returned string will start at the start'th position in string, counting from zero. For example, in the string 'abcdef', the character at position 0 is 'a', the character at position 2 is 'c', and so on. |
| `$length` | `int` | null | If length is given and is positive, the string returned will contain at most length characters starting from start (depending on the length of string). |


Return values
----------------

`string`

|bool the extracted part of string or false on failure.

Other resources
------------

[Official substr documentation](https://www.php.net/manual/en/function.substr.php)
