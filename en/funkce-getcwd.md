PHP function getcwd()
=====================

> id: '27e2d657-fa74-4686-b802-d73056c97e36'
> slug:
> 	cs: funkce-getcwd
> 	en: php-function-getcwd
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '489368ecda7f141540532aa88215a97a'

Availability in `PHP 4.0`

Gets the current working directory


Parameters
--------------

The function has no input parameters.

Return values
----------------

`string`

the current working directory on success, or false on
failure.
</p>
<p>
On some Unix variants, getcwd will return
false if any one of the parent directories does not have the
readable or search mode set, even if the current directory
does. See chmod for more information on
modes and permissions.

Additional resources
------------

[Official getcwd documentation](https://www.php.net/manual/en/function.getcwd.php)
