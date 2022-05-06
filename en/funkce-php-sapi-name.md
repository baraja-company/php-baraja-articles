PHP function php_sapi_name()
============================

> id: '0e464054-1f92-43fb-890b-5ea233d6c571'
> slug:
> 	cs: funkce-php-sapi-name
> 	en: php-function-php-sapi-name
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '208eede4f242078dde06544241ce9127'

Availability in versions: `PHP 4.0.1`

Returns the type of interface between web server and PHP


Parameters
--------------

The function has no input parameters.

Return values
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

Other resources
------------

[Official documentation for php-sapi-name](https://www.php.net/manual/en/function.php-sapi-name.php)
