PHP funkce sys_getloadavg()
===========================

> id: f6f28ae4-5623-4442-a6f8-6e6b46394504
> slug:
> 	cs: funkce-sys-getloadavg
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.1.3`

Získá průměrné zatížení systému. Stejné hodnoty, jako znáte z nástroje `htop` v Linuxu.

Příklad:

```php
$load = sys_getloadavg();
if ($load[0] > 0.80) { // je zátěž větší než 80 %?
    header('HTTP/1.1 503 Too busy, try again later');
    die('Server je přetížen, zkuste to později.');
}
```

Parametry
--------------

Funkce nemá žádné vstupní parametry.

Návratové hodnoty
----------------

`array`

pole se třemi vzorky (poslední 1 minuta, 5 minut a 15 minut).

Další zdroje
------------

[Oficiální dokumentace funkce sys-getloadavg](https://www.php.net/manual/en/function.sys-getloadavg.php)
