PHP funkce pack()
=================

> id: c3f6c522-a6aa-40e4-872b-5ff01d03fd1c
> slug:
> 	cs: funkce-pack
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Pack data into binary string


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$format` | `string` | *není* | The format string consists of format codes followed by an optional repeater argument. The repeater argument can be either an integer value or * for repeating to the end of the input data. For a, A, h, H the repeat count specifies how many characters of one data argument are taken, for @ it is the absolute position where to put the next data, for everything else the repeat count specifies how many data arguments are consumed and packed into the resulting binary string. |
| `$args` | `mixed` | null, |  |
| `$_` | `mixed` | null |  |


Návratové hodnoty
----------------

`string`

a binary string containing data.

Další zdroje
------------

https://www.php.net/manual/en/function.pack.php
