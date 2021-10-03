PHP funkce stream_select()
==========================

> id: "5dd24afa-fcf3-4ce5-8ee4-f8267d7efb02"
> slug:
> 	cs: funkce-stream-select
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.3.0`

Runs the equivalent of the select() system call on the given
arrays of streams with a timeout specified by tv_sec and tv_usec


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$read` | `array` |  | The streams listed in the read array will be watched to see if characters become available for reading (more precisely, to see if a read will not block - in particular, a stream resource is also ready on end-of-file, in which case an fread will return a zero length string). |
| `$write` | `array` |  | The streams listed in the write array will be watched to see if a write will not block. |
| `$except` | `array` |  | The streams listed in the except array will be watched for high priority exceptional ("out-of-band") data arriving. |
| `$tv_sec` | `int` |  | The tv_sec and tv_usec together form the timeout parameter, tv_sec specifies the number of seconds while tv_usec the number of microseconds. The timeout is an upper bound on the amount of time that stream_select will wait before it returns. If tv_sec and tv_usec are both set to 0, stream_select will not wait for data - instead it will return immediately, indicating the current status of the streams. |
| `$tv_usec` | `int` | null | See tv_sec description. |


Návratové hodnoty
----------------

`int`

On success stream_select returns the number of
stream resources contained in the modified arrays, which may be zero if
the timeout expires before anything interesting happens. On error false
is returned and a warning raised (this can happen if the system call is
interrupted by an incoming signal).

Další zdroje
------------

https://www.php.net/manual/en/function.stream-select.php
