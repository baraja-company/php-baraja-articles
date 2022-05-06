PHP function popen()
====================

> id: b49b0002-c900-4112-804e-89e046dd5de6
> slug:
> 	cs: funkce-popen
> 	en: php-function-popen
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '937c46146ce7ac71a4c0b0668745bf79'

Availability in `PHP 4.0`

Opens process file pointer


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$command` | `string` | *not* | The command |
| `$mode` | `string` | *not* | The mode |


Return values
----------------

`resource`

a file pointer identical to that returned by
fopen, except that it is unidirectional (may
only be used for reading or writing) and must be closed with
pclose. This pointer may be used with
fgets, fgetss, and
fwrite.
</p>
<p>
If an error occurs, returns false.

Other resources
------------

[Official popen documentation](https://www.php.net/manual/en/function.popen.php)
