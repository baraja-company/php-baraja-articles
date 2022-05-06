PHP function register_tick_function()
=====================================

> id: '9be6f817-3c51-4959-90c5-0eeee00e240a'
> slug:
> 	cs: funkce-register-tick-function
> 	en: php-function-register-tick-function
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: a69f5044ed8b93caa207d0af7b588a99

Availability in versions: `PHP 4.0.3`

Register a function for execution on each tick


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$function` | `callback` | *not* | The function name as a string, or an array consisting of an object and a method. |
| `$arg` | `mixed` | null, | | |
| `$_` | `mixed` | null | | |


Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.

Other resources
------------

[Official register-tick-function documentation](https://www.php.net/manual/en/function.register-tick-function.php)
