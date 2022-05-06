PHP function getrusage()
========================

> id: c19b63b6-232b-46f5-ade7-996308d5464f
> slug:
> 	cs: funkce-getrusage
> 	en: php-function-getrusage
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '0faa1f709e037a48e694b2e0b229de5c'

Availability in `PHP 4.0`

Gets the current resource usages


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$who` | `int` | null | If who is 1, getrusage will be called with RUSAGE_CHILDREN. |


Return values
----------------

`array`

an associative array containing the data returned from the system
call. All entries are accessible by using their documented field names.

Other resources
------------

[Official getrusage documentation](https://www.php.net/manual/en/function.getrusage.php)
