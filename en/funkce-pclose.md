PHP function pclose()
=====================

> id: dd52fa6f-c280-4639-a71c-665b0dcf7691
> slug:
> 	cs: funkce-pclose
> 	en: php-function-pclose
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '60128ce36d539d3201e94fefc7daa218'

Availability in `PHP 4.0`

Closes process file pointer


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$handle` | `resource` | *not* | The file pointer must be valid, and must have been returned by a successful call to popen. |


Return values
----------------

`int`

the termination status of the process that was run.

Other resources
------------

[Official pclose documentation](https://www.php.net/manual/en/function.pclose.php)
