PHP function ob_list_handlers()
===============================

> id: '7be89d03-94e9-4bb7-b454-2e0561eb073c'
> slug:
> 	cs: funkce-ob-list-handlers
> 	en: php-function-ob-list-handlers
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '35616ca58a6f5d9e6580a6856a4d205d'

Availability in versions: `PHP 4.3.0`

List all output handlers in use


Parameters
--------------

The function has no input parameters.

Return values
----------------

`array`

This will return an array with the output handlers in use (if any). If
output_buffering is enabled or
an anonymous function was used with ob_start,
ob_list_handlers will return "default output
handler".

Additional resources
------------

[Official ob-list-handlers documentation](https://www.php.net/manual/en/function.ob-list-handlers.php)
