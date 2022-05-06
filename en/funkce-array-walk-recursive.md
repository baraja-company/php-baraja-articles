PHP function array_walk_recursive()
===================================

> id: e71c1de4-4333-4d8d-9160-8c0d30481f46
> slug:
> 	cs: funkce-array-walk-recursive
> 	en: php-function-array-walk-recursive
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '59874540-664b-4474-9869-7e6742ab6051'
> sourceContentHash: c3bfa9053e0fdaf823eea6a4da6ecb81

Availability in `PHP 5.0`

Apply a user function recursively to every member of an array


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$input` | `array` | *not* | The input array. |
| `$funcname` | `callback` | *not* | Typically, funcname takes on two parameters. The input parameter's value being the first, and the key/index second. |
| `$userdata` | `mixed` | null | If the optional userdata parameter is supplied, it will be passed as the third parameter to the callback funcname. |


Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.

Other resources
------------

[Official documentation for the array-walk-recursive function](https://www.php.net/manual/en/function.array-walk-recursive.php)
