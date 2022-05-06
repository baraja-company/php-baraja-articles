PHP function readdir()
======================

> id: '3f7538f0-d1f2-4938-a58e-0086cba907b3'
> slug:
> 	cs: funkce-readdir
> 	en: php-function-readdir
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '843682cc72dd206496a53aef5468cc8c'

Availability in `PHP 4.0`

Read entry from directory handle


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$dir_handle` | `resource` | null | The directory handle resource previously opened with opendir. If the directory handle is not specified, the last link opened by opendir is assumed. |


Return values
----------------

`string`

|bool the filename on success or false on failure.

Other resources
------------

[Official readdir documentation](https://www.php.net/manual/en/function.readdir.php)
