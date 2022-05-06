PHP function addcslashes()
==========================

> id: caa941b6-05e0-426f-97b7-f969c7874638
> slug:
> 	cs: funkce-addcslashes
> 	en: php-function-addcslashes
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '38142bffde52ee6cdbbeed20602271f5'

Availability in `PHP 4.0`

Quote string with slashes in a C style


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$str` | `string` | *not* | String to be escaped. |
| `$charlist` | `string` | *not* | List of characters to be escaped. If charlist contains `\n`, `\r`, etc., they are converted to C style, while other non-alphanumeric characters with ASCII codes lower than 32 and higher than 126 are converted to octal representation. |


Return values
----------------

`string`

the escaped string.

Other resources
------------

[Official addcslashes documentation](https://www.php.net/manual/en/function.addcslashes.php)
