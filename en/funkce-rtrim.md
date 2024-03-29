PHP function rtrim()
====================

> id: e8b81e57-fadb-417e-a67f-ee895fbaa043
> slug:
> 	cs: funkce-rtrim
> 	en: php-function-rtrim
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '5e4985ebf95af1184d9e7cfbe9efcd87'

Availability in `PHP 4.0`

Strip whitespace (or other characters) from the end of a string.
Without the second parameter, rtrim() will strip these characters:
<ul>
<li>" " (ASCII 32 (0x20)), an ordinary space.
<li>"\t" (ASCII 9 (0x09)), a tab.
<li>"\n" (ASCII 10 (0x0A)), a new line (line feed).
<li>"\r" (ASCII 13 (0x0D)), a carriage return.
<li>"\0" (ASCII 0 (0x00)), the NUL-byte.
<li>"\x0B" (ASCII 11 (0x0B)), a vertical tab.
</ul>


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$str` | `string` | *not* | String being processed. |
| `$charlist` | `string` | " | You can also specify the characters you want to strip, by means of the charlist parameter. Simply list all characters that you want to be stripped. With .. you can specify a range of characters. |


Return values
----------------

`string`

A modified string.

Other resources
------------

[Official rtrim documentation](https://www.php.net/manual/en/function.rtrim.php)
