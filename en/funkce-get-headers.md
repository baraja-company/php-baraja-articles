PHP function get_headers()
==========================

> id: c82bbb88-1150-48f6-921b-ede602aaf184
> slug:
> 	cs: funkce-get-headers
> 	en: php-function-get-headers
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: efbd8f4eb095fe4636ba3296d4978ce9

Availability in `PHP 5.0`

Fetches all the headers sent by the server in response to a HTTP request


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$url` | `string` | *not* | The target URL. |
| `$format` | `int` | null, | If the optional format parameter is set to non-zero, get_headers parses the response and sets the array's keys. |
| | `$context` | `resource` | *not* | |


Return values
----------------

`array`

an indexed or associative array with the headers, or false on
failure.

Other resources
------------

[Official get-headers documentation](https://www.php.net/manual/en/function.get-headers.php)
