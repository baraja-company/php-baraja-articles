PHP function register_shutdown_function()
=========================================

> id: '1d74db45-e5a3-457c-8530-c4e52e9d4c06'
> slug:
> 	cs: funkce-register-shutdown-function
> 	en: php-function-register-shutdown-function
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '0f12291a0c74b4b83ae252d671706851'

Availability in `PHP 4.0`

Register a function for execution on shutdown


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$function` | `callback` | *not* | The shutdown function to register. |
| `$parameter` | `mixed` | null, | It is possible to pass parameters to the shutdown function by passing additional parameters. |
| `$_` | `mixed` | null | | |


Return values
----------------

`void`



Other resources
------------

[Register-shutdown-function official documentation](https://www.php.net/manual/en/function.register-shutdown-function.php)
