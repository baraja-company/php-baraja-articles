PHP funkce stream_socket_enable_crypto()
========================================

> id: fb15ad4f-83e8-4d78-96ba-389292a9987e
> slug:
> 	cs: funkce-stream-socket-enable-crypto
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.1.0`

Turns encryption on/off on an already connected socket


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$stream` | `resource` |  | The stream resource. |
| `$enable` | `bool` |  | Enable/disable cryptography on the stream. |
| `$crypto_type` | `int` | null, | Setup encryption on the stream. Valid methods are STREAM_CRYPTO_METHOD_SSLv2_CLIENT @param resource $session_stream [optional] <p> Seed the stream with settings from session_stream. |
| `$session_stream` | `resource` | null | Seed the stream with settings from session_stream. |


Návratové hodnoty
----------------

`mixed`

true on success, false if negotiation has failed or
0 if there isn't enough data and you should try again
(only for non-blocking sockets).

Další zdroje
------------

https://www.php.net/manual/en/function.stream-socket-enable-crypto.php
