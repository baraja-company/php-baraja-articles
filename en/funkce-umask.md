PHP function umask()
====================

> id: a8367c75-5190-4055-a7bd-6e37ade7020f
> slug:
> 	cs: funkce-umask
> 	en: php-function-umask
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '8cf570497216c0082e47566ad7a65709'

Availability in `PHP 4.0`

changes the current umask


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$mask` | `int` | null | New umask. |


Return values
----------------

`int`

umask without arguments simply returns, otherwise the old umask is returned.

Additional resources
------------

[Official umask documentation](https://www.php.net/manual/en/function.umask.php)
