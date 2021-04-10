PHP funkce stream_socket_recvfrom()
===================================

> id: "1f707e7f-06e7-4a89-8931-60d12e052726"
> slug:
> 	cs: funkce-stream-socket-recvfrom
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.0`

Receives data from a socket, connected or not


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$socket` | `resource` |  | The remote socket. |
| `$length` | `int` |  | The number of bytes to receive from the socket. |
| `$flags` | `int` | null, | The value of flags can be any combination of the following: <table> Possible values for flags <tr valign="top"> <td>STREAM_OOB</td> <td> Process OOB (out-of-band) data. </td> </tr> <tr valign="top"> <td>STREAM_PEEK</td> <td> Retrieve data from the socket, but do not consume the buffer. Subsequent calls to fread or stream_socket_recvfrom will see the same data. </td> </tr> </table> |
| `$address` | `string` | null | If address is provided it will be populated with the address of the remote socket. |


Návratové hodnoty
----------------

`string`

the read data, as a string

Další zdroje
------------

https://php.net/manual/en/function.stream-socket-recvfrom.php
