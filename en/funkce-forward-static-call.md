PHP function forward_static_call()
==================================

> id: caa37bf0-ad88-47ff-b647-045ff708660e
> slug:
> 	cs: funkce-forward-static-call
> 	en: php-function-forward-static-call
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '3964c18f3f14e79d8b17340cdd404467'

Availability in versions: `PHP 5.3.0`

Call a static method


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$function` | `callback` | *not* | The function or method to be called. This parameter may be an array, with the name of the class, and the method, or a string, with a function name. |
| `$parameter` | `mixed` | null, | Zero or more parameters to be passed to the function. |
| `$_` | `mixed` | null | |


Return values
----------------

`mixed`

the function result, or false on error.

Other resources
------------

[Forward-static-call official documentation](https://www.php.net/manual/en/function.forward-static-call.php)
