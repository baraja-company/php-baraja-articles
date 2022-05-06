PHP function chmod()
====================

> id: bf778176-8523-4fc2-9a6a-7d08badb1deb
> slug:
> 	cs: funkce-chmod
> 	en: php-function-chmod
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: d4433c9c12389d20a86e63f3b5307ceb

Availability in `PHP 4.0`

Sets file access rights (read, write, delete).

Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | File path (absolute is recommended) |
| `$mode` | `int` | *not* | Access rights (for example `0777` - everyone has access). Note: Rights will be automatically converted to hexadecimal. Writing as a string (for example `g+w` is not supported). If you want to enter a number in decimal, enter zero as the first digit (for example `0654`).


Return values
----------------

`bool`

Returns `true` if everything went fine, `false` if an error occurred.

Additional resources
------------

[Official chmod documentation](https://www.php.net/manual/en/function.chmod.php)
