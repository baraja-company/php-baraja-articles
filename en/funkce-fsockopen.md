PHP function fsockopen()
========================

> id: '42b0b00e-f3ce-48bd-960d-c269dd41b700'
> slug:
> 	cs: funkce-fsockopen
> 	en: php-function-fsockopen
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: ec7bc8b5c442dcc016846d958c059681

Availability in `PHP 4.0`

Open Internet or Unix domain socket connection


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$hostname` | `string` | *not* | If you have compiled in OpenSSL support, you may prefix the hostname with either ssl:// or tls:// to use an SSL or TLS client connection over TCP/IP to connect to the remote host. |
| `$port` | `int` | null, | The port number. |
| `$errno` | `int` | null, | If provided, holds the system level error number that occurred in the system-level connect() call. |
| | `$errstr` | `string` | null, | The error message as a string. |
| | `$timeout` | `float` | null | The connection timeout, in seconds. |


Return values
----------------

`resource`

fsockopen returns a file pointer which may be used
together with the other file functions (such as
fgets, fgetss,
fwrite, fclose, and
feof). If the call fails, it will return false

Other resources
------------

[Official fsockopen documentation](https://www.php.net/manual/en/function.fsockopen.php)
