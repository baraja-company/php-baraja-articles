PHP function lstat()
====================

> id: '9deef082-98ec-4d2b-8599-133f107f5155'
> slug:
> 	cs: funkce-lstat
> 	en: php-function-lstat
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '99258f7034eef37d4790590fa90b3b32'

Availability in `PHP 4.0`

Gives information about a file or symbolic link


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | Path to a file or a symbolic link. |


Return values
----------------

`array`

See the manual page for stat for information on
the structure of the array that lstat returns.
This function is identical to the stat function
except that if the filename parameter is a symbolic
link, the status of the symbolic link is returned, not the status of the
file pointed to by the symbolic link.

Other resources
------------

[Official lstat documentation](https://www.php.net/manual/en/function.lstat.php)
