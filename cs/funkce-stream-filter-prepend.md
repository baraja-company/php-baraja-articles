PHP funkce stream_filter_prepend()
==================================

> id: a9f6d6d8-32ac-4846-a15d-c357f9dc1e26
> slug:
> 	cs: funkce-stream-filter-prepend
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.3.0`

Attach a filter to a stream


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$stream` | `resource` |  | The target stream. |
| `$filtername` | `string` |  | The filter name. |
| `$read_write` | `int` | null, | By default, stream_filter_prepend will attach the filter to the read filter chain if the file was opened for reading (i.e. File Mode: r, and/or +). The filter will also be attached to the write filter chain if the file was opened for writing (i.e. File Mode: w, a, and/or +). STREAM_FILTER_READ, STREAM_FILTER_WRITE, and/or STREAM_FILTER_ALL can also be passed to the read_write parameter to override this behavior. See stream_filter_append for an example of using this parameter. |
| `$params` | `mixed` | null | This filter will be added with the specified params to the beginning of the list and will therefore be called first during stream operations. To add a filter to the end of the list, use stream_filter_append. |


Návratové hodnoty
----------------

`resource`

a resource which can be used to refer to this filter
instance during a call to stream_filter_remove.

Další zdroje
------------

https://php.net/manual/en/function.stream-filter-prepend.php
