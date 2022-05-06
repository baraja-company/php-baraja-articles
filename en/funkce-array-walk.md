PHP function array_walk()
=========================

> id: '9a37b2cc-6123-404b-ad12-8c430cbf1f14'
> slug:
> 	cs: funkce-array-walk
> 	en: php-function-array-walk
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '59874540-664b-4474-9869-7e6742ab6051'
> sourceContentHash: '7e8f4294405b7cb47fd238b03e4d5740'

Availability in `PHP 4.0`

Apply a user function to every member of an array


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$array` | `array` | *not* | The input array. |
| `$funcname` | `callback` | *not* | Typically, funcname takes on two parameters. The array parameter's value being the first, and the key/index second. |
| `$userdata` | `mixed` | null | If the optional userdata parameter is supplied, it will be passed as the third parameter to the callback funcname. |


Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.

Other resources
------------

[Official documentation for the array-walk function](https://www.php.net/manual/en/function.array-walk.php)
