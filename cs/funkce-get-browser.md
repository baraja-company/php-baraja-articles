PHP funkce get_browser()
========================

> id: "7a4c99e8-dbf9-46e0-bee5-933ee2bc2272"
> slug:
> 	cs: funkce-get-browser
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Tells what the user's browser is capable of


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$user_agent` | `string` | null, | The User Agent to be analyzed. By default, the value of HTTP User-Agent header is used; however, you can alter this (i.e., look up another browser's info) by passing this parameter. |
| `$return_array` | `bool` | null | If set to true, this function will return an array instead of an object. |


Návratové hodnoty
----------------

`mixed`

The information is returned in an object or an array which will contain
various data elements representing, for instance, the browser's major and
minor version numbers and ID string; true/false values for features
such as frames, JavaScript, and cookies; and so forth.
</p>
<p>
The cookies value simply means that the browser
itself is capable of accepting cookies and does not mean the user has
enabled the browser to accept cookies or not. The only way to test if
cookies are accepted is to set one with setcookie,
reload, and check for the value.

Další zdroje
------------

[Oficiální dokumentace funkce get-browser](https://www.php.net/manual/en/function.get-browser.php)
