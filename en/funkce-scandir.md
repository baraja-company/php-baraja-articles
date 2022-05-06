PHP function scandir()
======================

> id: c4da3f63-82a0-424c-90d8-eecd1e5577d2
> slug:
> 	cs: funkce-scandir
> 	en: php-function-scandir
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '8623d96f883d80a59cfaff6e7733a5e5'

Availability in `PHP 5.0`

List files and directories inside the specified path


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$directory` | `string` | *not* | The directory that will be scanned. |
| `$sorting_order` | `int` | null, | By default, the sorted order is alphabetical in ascending order. If the optional sorting_order is set to non-zero, then the sort order is alphabetical in descending order. |
| `$context` | `resource` | null | For a description of the context parameter, refer to the streams section of the manual. |


Return values
----------------

`array`

an array of filenames on success, or false on
failure. If directory is not a directory, then
boolean false is returned, and an error of level
E_WARNING is generated.

Additional resources
------------

[Official scandir documentation](https://www.php.net/manual/en/function.scandir.php)
