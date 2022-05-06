PHP function filegroup()
========================

> id: '6429ca80-161b-4299-9a0b-e7ac4b715923'
> slug:
> 	cs: funkce-filegroup
> 	en: php-function-filegroup
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '291a162e3e7403d9ee4b61c6d8cf2359'

Availability in `PHP 4.0`

Gets file group


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | Path to the file. |


Return values
----------------

`int`

|bool the group ID of the file, or false in case
of an error. The group ID is returned in numerical format, use
posix_getgrgid to resolve it to a group name.
Upon failure, false is returned.

Additional resources
------------

[Official filegroup documentation](https://www.php.net/manual/en/function.filegroup.php)
