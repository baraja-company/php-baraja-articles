PHP function ob_end_clean()
===========================

> id: '5acd88fb-ec0d-4122-bbcd-38377e3b9394'
> slug:
> 	cs: funkce-ob-end-clean
> 	en: php-function-ob-end-clean
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '129e3e1b6c1f714af89882c9bc3ce60c'

Availability in `PHP 4.0`

Clean (erase) the output buffer and turn off output buffering


Parameters
--------------

The function has no input parameters.

Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure. Reasons for failure are first that you called the
function without an active buffer or that for some reason a buffer could
not be deleted (possible for special buffer).

Other resources
------------

[Official ob-end-clean documentation](https://www.php.net/manual/en/function.ob-end-clean.php)
