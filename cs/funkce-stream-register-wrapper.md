PHP funkce stream_register_wrapper()
====================================

> id: c05704b9-26dd-4918-acb7-520519c8b3e8
> slug:
> 	cs: funkce-stream-register-wrapper
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.3.0`

&Alias; <function>stream_wrapper_register</function>
<p>Register a URL wrapper implemented as a PHP class


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$protocol` | `string` | *není* | The wrapper name to be registered. |
| `$classname` | `string` | *není* | The classname which implements the protocol. |
| `$flags` | `int` | *není* | Should be set to STREAM_IS_URL if protocol is a URL protocol. Default is 0, local stream. |


Návratové hodnoty
----------------

`bool`

vrátí `true` v případě úspěchu, jinak `false` v případě neúspěchu.
</p>
<p>
stream_wrapper_register will return false if the
protocol already has a handler.

Další zdroje
------------

[Oficiální dokumentace funkce stream-register-wrapper](https://www.php.net/manual/en/function.stream-register-wrapper.php)
