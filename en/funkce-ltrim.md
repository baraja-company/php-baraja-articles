PHP function ltrim()
====================

> id: a25d0805-ff9f-42f4-968e-7666ce89e8d9
> slug:
> 	cs: funkce-ltrim
> 	en: php-function-ltrim
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '3522c41c23e4da8596c4080eb23b3cdb'

Availability in `PHP 4.0`

Strip whitespace (or other characters) from the beginning of a string


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$str` | `string` | *not* | String being processed. |
| `$charlist` | `string` | " | You can also specify the characters you want to strip, by means of the charlist parameter. Simply list all characters that you want to be stripped. With .. you can specify a range of characters. |


Return values
----------------

`string`

This function returns a string with whitespace stripped from the
beginning of str.
Without the second parameter,
ltrim will strip these characters:
" " (ASCII 32
(0x20)), an ordinary space.
"\t" (ASCII 9
(0x09)), a tab.
"\n" (ASCII 10
(0x0A)), a new line (line feed).
"\r" (ASCII 13
(0x0D)), a carriage return.
"\0" (ASCII 0
(0x00)), the NUL-byte.
"\x0B" (ASCII 11
(0x0B)), a vertical tab.

Other resources
------------

[Official ltrim documentation](https://www.php.net/manual/en/function.ltrim.php)
