PHP funkce checkdnsrr()
=======================

> id: "9e26c84c-6700-46c1-a87b-a1de4797244f"
> slug:
> 	cs: funkce-checkdnsrr
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Check DNS records corresponding to a given Internet host name or IP address


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$host` | `string` | *není* | host may either be the IP address in dotted-quad notation or the host name. |
| `$type` | `string` | null | type may be any one of: A, MX, NS, SOA, PTR, CNAME, AAAA, A6, SRV, NAPTR, TXT or ANY. |


Návratové hodnoty
----------------

`bool`

true if any records are found; returns false if no records
were found or if an error occurred.

Další zdroje
------------

https://www.php.net/manual/en/function.checkdnsrr.php
