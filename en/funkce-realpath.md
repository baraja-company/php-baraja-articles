PHP function realpath()
=======================

> id: '28a7eaf9-5c2e-4933-b701-5637ea0857ab'
> slug:
> 	cs: funkce-realpath
> 	en: php-function-realpath
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: ca3ae593548775332eceff3ff99accaa

Availability in `PHP 4.0`

Returns canonicalized absolute pathname


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$path` | `string` | *not* | The path being checked. |


Return values
----------------

`string`

|bool the canonicalized absolute pathname on success. The resulting path
will have no symbolic link, '/./' or '/../' components.
</p>
<p>
realpath returns false on failure, e.g. if
the file does not exist.

Additional resources
------------

[Official realpath documentation](https://www.php.net/manual/en/function.realpath.php)
