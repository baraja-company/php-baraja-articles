PHP funkce socket_get_status()
==============================

> id: "4db2246a-a323-4ea9-a5ec-7de9e85a993d"
> slugCS: funkce-socket-get-status
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.3.0`

&Alias; <function>stream_get_meta_data</function>
Retrieves header/meta data from streams/file pointers


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$stream` | `resource` |  | The stream can be any stream created by fopen, fsockopen and pfsockopen. |


Návratové hodnoty
----------------

`array`

The result array contains the following items:
</p>
<p>
timed_out (bool) - true if the stream
timed out while waiting for data on the last call to
fread or fgets.
</p>
<p>
blocked (bool) - true if the stream is
in blocking IO mode. See stream_set_blocking.
</p>
<p>
eof (bool) - true if the stream has reached
end-of-file. Note that for socket streams this member can be true
even when unread_bytes is non-zero. To
determine if there is more data to be read, use
feof instead of reading this item.
</p>
<p>
unread_bytes (int) - the number of bytes
currently contained in the PHP's own internal buffer.
</p>
You shouldn't use this value in a script.
<p>
stream_type (string) - a label describing
the underlying implementation of the stream.
</p>
<p>
wrapper_type (string) - a label describing
the protocol wrapper implementation layered over the stream.
See for more information about wrappers.
</p>
<p>
wrapper_data (mixed) - wrapper specific
data attached to this stream. See for
more information about wrappers and their wrapper data.
</p>
<p>
filters (array) - and array containing
the names of any filters that have been stacked onto this stream.
Documentation on filters can be found in the
Filters appendix.
</p>
<p>
mode (string) - the type of access required for
this stream (see Table 1 of the fopen() reference)
</p>
<p>
seekable (bool) - whether the current stream can
be seeked.
</p>
<p>
uri (string) - the URI/filename associated with this
stream.

Další zdroje
------------

https://php.net/manual/en/function.socket-get-status.php
