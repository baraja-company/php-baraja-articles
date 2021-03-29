PHP funkce dl()
===============

> id: f25b7380-37f0-4e45-8df8-ecfa6df64415
> slugCS: funkce-dl
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$library` | `string` |  | This parameter is only the filename of the extension to load which also depends on your platform. For example, the sockets extension (if compiled as a shared module, not the default!) would be called sockets.so on Unix platforms whereas it is called php_sockets.dll on the Windows platform. |


Návratové hodnoty
----------------

`bool`

<b>TRUE</b> on success or <b>FALSE</b> on failure. If the functionality of loading modules is not available
or has been disabled (either by setting
enable_dl off or by enabling safe mode
in <i>php.ini</i>) an <b>E_ERROR</b> is emitted
and execution is stopped. If <b>dl</b> fails because the
specified library couldn't be loaded, in addition to <b>FALSE</b> an
<b>E_WARNING</b> message is emitted.

Další zdroje
------------

https://php.net/manual/en/function.dl.php
