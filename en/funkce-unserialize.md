PHP function unserialize()
==========================

> id: '2a898eba-4e92-4d6b-ac54-618ff790cde7'
> slug:
> 	cs: funkce-unserialize
> 	en: php-function-unserialize
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '48367f48127afb87cd31fd16671b8717'

Availability in `PHP 4.0`, `PHP 7.0`

Creates a PHP value from a stored representation


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$str` | `string` | *not* | Serialized string. |
| `$options` | `mixed` | null | |


Return values
----------------

`mixed`

Returns the converted value, which can be boolean, integer, float, string, array or object.

If the passed string is not unserializable, false is returned and `E_NOTICE` is issued.

Other resources
------------

[Official unserialize function documentation](https://www.php.net/manual/en/function.unserialize.php)
