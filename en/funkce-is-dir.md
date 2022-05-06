PHP function is_dir()
=====================

> id: '257cc0c1-c7da-4f9b-8c4a-21ecaba5dafd'
> slug:
> 	cs: funkce-is-dir
> 	en: php-function-is-dir
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: af15d8fa1e506fccc8ec1b5c825d0e11

Availability in `PHP 4.0`

Tells whether the filename is a directory


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | Path to the file. If filename is a relative filename, it will be checked relative to the current working directory. If filename is a symbolic or hard link then the link will be resolved and checked. |


Return values
----------------

`bool`

true if the filename exists and is a directory, false
otherwise.

Other resources
------------

[Official is-dir documentation](https://www.php.net/manual/en/function.is-dir.php)
