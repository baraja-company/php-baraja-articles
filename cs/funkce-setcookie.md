PHP funkce setcookie()
======================

> id: f3d57a13-921b-4d45-b2ac-8dbd44cf1a89
> slug:
> 	cs: funkce-setcookie
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Send a cookie


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$name` | `string` | *není* | The name of the cookie. |
| `$value` | `string` | null, | The value of the cookie. This value is stored on the clients computer; do not store sensitive information. Assuming the name is 'cookiename', this value is retrieved through $_COOKIE['cookiename'] |
| `$expire` | `int` | null, | The time the cookie expires. This is a Unix timestamp so is in number of seconds since the epoch. In other words, you'll most likely set this with the time function plus the number of seconds before you want it to expire. Or you might use mktime. time()+60*60*24*30 will set the cookie to expire in 30 days. If set to 0, or omitted, the cookie will expire at the end of the session (when the browser closes). |
| `$path` | `string` | null, | The path on the server in which the cookie will be available on. If set to '/', the cookie will be available within the entire domain. If set to '/foo/', the cookie will only be available within the /foo/ directory and all sub-directories such as /foo/bar/ of domain. The default value is the current directory that the cookie is being set in. |
| `$domain` | `string` | null, | The domain that the cookie is available. To make the cookie available on all subdomains of example.com then you'd set it to '.example.com'. The . is not required but makes it compatible with more browsers. Setting it to www.example.com will make the cookie only available in the www subdomain. Refer to tail matching in the spec for details. |
| `$secure` | `bool` | null, | Indicates that the cookie should only be transmitted over a secure HTTPS connection from the client. When set to true, the cookie will only be set if a secure connection exists. On the server-side, it's on the programmer to send this kind of cookie only on secure connection (e.g. with respect to $_SERVER["HTTPS"]). |
| `$httponly` | `bool` | null | When true the cookie will be made accessible only through the HTTP protocol. This means that the cookie won't be accessible by scripting languages, such as JavaScript. This setting can effectively help to reduce identity theft through XSS attacks (although it is not supported by all browsers). Added in PHP 5.2.0. true or false |


Návratové hodnoty
----------------

`bool`

If output exists prior to calling this function,
setcookie will fail and return false. If
setcookie successfully runs, it will return true.
This does not indicate whether the user accepted the cookie.

Další zdroje
------------

[Oficiální dokumentace funkce setcookie](https://www.php.net/manual/en/function.setcookie.php)
