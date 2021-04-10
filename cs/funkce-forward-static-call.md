PHP funkce forward_static_call()
================================

> id: caa37bf0-ad88-47ff-b647-045ff708660e
> slug:
> 	cs: funkce-forward-static-call
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.3.0`

Call a static method


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$function` | `callback` |  | The function or method to be called. This parameter may be an array, with the name of the class, and the method, or a string, with a function name. |
| `$parameter` | `mixed` | null, | Zero or more parameters to be passed to the function. |
| `$_` | `mixed` | null |  |


Návratové hodnoty
----------------

`mixed`

the function result, or false on error.

Další zdroje
------------

https://php.net/manual/en/function.forward-static-call.php
