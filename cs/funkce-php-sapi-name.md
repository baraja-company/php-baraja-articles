PHP funkce php_sapi_name()
================================

> id: 0e464054-1f92-43fb-890b-5ea233d6c571
> slugCS: funkce-php-sapi-name
> publicationDate: 2019-09-11 10:04:03
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 4.0.1`

Returns the type of interface between web server and PHP


Parametry
--------------

Funkce nemá žádné vstupní parametry.

Návratové hodnoty
----------------

`string`

the interface type, as a lowercase string.
</p>
<p>
Although not exhaustive, the possible return values include
aolserver, apache,
apache2filter, apache2handler,
caudium, cgi (until PHP 5.3),
cgi-fcgi, cli,
continuity, embed,
isapi, litespeed,
milter, nsapi,
phttpd, pi3web, roxen,
thttpd, tux, and webjames.

Další zdroje
------------

https://php.net/manual/en/function.php-sapi-name.php
