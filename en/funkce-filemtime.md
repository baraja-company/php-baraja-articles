PHP function filemtime()
========================

> id: '7e2012a8-2358-4ac7-b827-91fffb1e793b'
> slug:
> 	cs: funkce-filemtime
> 	en: php-function-filemtime
> 
> publicationDate: '2019-09-11 10:04:00'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '5459c8ebb272c6173b8e276945f1ba6e'

Availability in `PHP 4.0`

Gets file modification time


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | Path to the file. |


Return values
----------------

`int`

|bool the time the file was last modified, or false on failure.
The time is returned as a Unix timestamp, which is
suitable for the date function.

Other resources
------------

[Official filetime function documentation](https://www.php.net/manual/en/function.filemtime.php)
