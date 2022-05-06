PHP function lchgrp()
=====================

> id: '6f032627-0739-4057-a948-4982d24b2b5b'
> slug:
> 	cs: funkce-lchgrp
> 	en: php-function-lchgrp
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: bd9ffeffe0a9feeecf046b5ccb826d83

Availability in versions: `PHP 5.1.2`

Changes group ownership of symlink


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | Path to the symlink. |
| `$group` | `mixed` | *not* | The group specified by name or number. |


Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.

Other resources
------------

[Official lchgrp documentation](https://www.php.net/manual/en/function.lchgrp.php)
