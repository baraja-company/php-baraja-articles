PHP function touch()
====================

> id: '8fa0546d-5d5c-4597-8387-3d9b83115a8e'
> slug:
> 	cs: funkce-touch
> 	en: php-function-touch
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '78799b742b126bc3d0970a0b09513fce'

Availability in `PHP 4.0`

Sets file access and modification time


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | The name of the file you are touching. |
| `$time` | `int` | null, | time of touch. If no time is given, the current system time is used. |
| `$atime` | `int` | null | If specified, the time to access the specified filename is set to atime. Otherwise, it is set to time. |


Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.

Other resources
------------

[Official documentation for the touch function](https://www.php.net/manual/en/function.touch.php)
