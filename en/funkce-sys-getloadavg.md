PHP function sys_getloadavg()
=============================

> id: f6f28ae4-5623-4442-a6f8-6e6b46394504
> slug:
> 	cs: funkce-sys-getloadavg
> 	en: php-function-sys-getloadavg
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '33d1f175162122fc49d277a43360d18d'

Version availability: `PHP 5.1.3`

Gets the average system load. Same values as you know from the `htop` tool in Linux.

Example:

```php
$load = sys_getloadavg();
if ($load[0] > 0.80) { // is the load greater than 80%?
    header('HTTP/1.1 503 Too busy, try again later');
    die('Server is overloaded, try again later.');
}
```

Parameters
--------------

The function has no input parameters.

Return values
----------------

`array`

Array with three samples (last 1 minute, 5 minutes and 15 minutes).

Other resources
------------

[Official sys-getloadavg documentation](https://www.php.net/manual/en/function.sys-getloadavg.php)
