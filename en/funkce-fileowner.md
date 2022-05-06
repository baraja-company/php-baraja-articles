PHP function fileowner()
========================

> id: '64e4aa1d-2b93-406f-982d-dc98ff1ab042'
> slug:
> 	cs: funkce-fileowner
> 	en: php-function-fileowner
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '761759ae7bedc13c577612c689b8e5cd'

Availability in `PHP 4.0`

Gets file owner


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | Path to the file. |


Return values
----------------

`int`

|bool the user ID of the owner of the file, or false on failure.
The user ID is returned in numerical format, use
posix_getpwuid to resolve it to a username.

Other resources
------------

[Official fileowner documentation](https://www.php.net/manual/en/function.fileowner.php)
