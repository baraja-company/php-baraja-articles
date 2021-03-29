PHP funkce stream_socket_sendto()
=================================

> id: "4e137a2c-2235-4a88-af51-d1f0439b1c50"
> slugCS: funkce-stream-socket-sendto
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.0`

Sends a message to a socket, whether it is connected or not


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$socket` | `resource` |  | The socket to send data to. |
| `$data` | `string` |  | The data to be sent. |
| `$flags` | `int` | null, | The value of flags can be any combination of the following: <table> possible values for flags <tr valign="top"> <td>STREAM_OOB</td> <td> Process OOB (out-of-band) data. </td> </tr> </table> |
| `$address` | `string` | null | The address specified when the socket stream was created will be used unless an alternate address is specified in address. |


Návratové hodnoty
----------------

`int`

a result code, as an integer.

Další zdroje
------------

https://php.net/manual/en/function.stream-socket-sendto.php
