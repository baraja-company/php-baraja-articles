PHP function fclose()
=====================

> id: b3724c46-cbdf-44a9-998a-6d51ed22b3c2
> slug:
> 	cs: funkce-fclose
> 	en: php-function-fclose
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '374aa82f922162d27b396b79c1d5fa35'

Availability in `PHP 4.0`

Closes an open file pointer


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$handle` | `resource` | *not* | The file pointer must be valid, and must point to a file successfully opened by fopen or fsockopen. |


Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.

Other resources
------------

[Official fclose documentation](https://www.php.net/manual/en/function.fclose.php)
