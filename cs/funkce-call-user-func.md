PHP funkce call_user_func()
===========================

> id: "994791e4-966a-4f32-b72d-c236177cbe0d"
> slugCS: funkce-call-user-func
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Call a user function given by the first parameter


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$function` | `callback` |  | The function to be called. Class methods may also be invoked statically using this function by passing array($classname, $methodname) to this parameter. Additionally class methods of an object instance may be called by passing array($objectinstance, $methodname) to this parameter. |
| `$parameter` | `mixed` | null, | Zero or more parameters to be passed to the function. |
| `$_` | `mixed` | null |  |


Návratové hodnoty
----------------

`mixed`

the function result, or false on error.

Další zdroje
------------

https://php.net/manual/en/function.call-user-func.php
