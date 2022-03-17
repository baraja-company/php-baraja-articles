PHP funkce header_register_callback()
=====================================

> id: a7ab61c6-41fc-4ce5-b4bc-ba3ea34f15ee
> slug:
> 	cs: funkce-header-register-callback
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Registers a function that will be called when PHP starts sending output.
The callback is executed just after PHP prepares all headers to be sent,<br>
and before any other output is sent, creating a window to manipulate the outgoing headers before being sent.


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$callback` | `callable` |  |  |


Návratové hodnoty
----------------

`bool`

true on success or false on failure.

Další zdroje
------------

[Oficiální dokumentace funkce header-register-callback](https://www.php.net/manual/en/function.header-register-callback.php)
