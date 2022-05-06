PHP function php_ini_scanned_files()
====================================

> id: b1e9471e-1af6-476f-be1f-0a7c46c5823e
> slug:
> 	cs: funkce-php-ini-scanned-files
> 	en: php-function-php-ini-scanned-files
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '22d3cb6aef1f0fb9c71c0ed6805a02cd'

Availability in versions: `PHP 4.3.0`

Return a list of .ini files parsed from the additional ini dir


Parameters
--------------

The function has no input parameters.

Return values
----------------

`string`

A comma-separated string of .ini files on success. Each comma is
followed by a newline. If the directive --with-config-file-scan-dir wasn't set,
false is returned. If it was set and the directory was empty, an
empty string is returned. If a file is unrecognizable, the file will
still make it into the returned string but a PHP error will also result.
This PHP error will be seen both at compile time and while using
php_ini_scanned_files.

Other resources
------------

[Official documentation for php-ini-scanned-files](https://www.php.net/manual/en/function.php-ini-scanned-files.php)
