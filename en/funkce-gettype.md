PHP function gettype()
======================

> id: ea1107b4-1a9f-497f-8e75-9e17ce4ca4c4
> slug:
> 	cs: funkce-gettype
> 	en: php-function-gettype
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '86816c190bc3c76bf9cc66a10a410fba'

Availability in `PHP 4.0`

Returns the type of the variable.

Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$var` | `mixed` | *not* | Variable to check |


Return values
----------------

`string`

Returns the type of the variable.

Returns one of the following:

- `boolean`
- `integer`
- `double` (for historical reasons, `double` is returned instead of `float`, based on the C implementation of PHP, which uses `double`)
- `string`
- `array`
- `object`
- `resource`
- `NULL`
- `unknown type` (unknown type)

Other resources
------------

[Official gettype documentation](https://www.php.net/manual/en/function.gettype.php)
