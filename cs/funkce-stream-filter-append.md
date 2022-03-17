PHP funkce stream_filter_append()
=================================

> id: "884e9069-ed27-407c-8320-e0aed9a01466"
> slug:
> 	cs: funkce-stream-filter-append
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.3.0`

Attach a filter to a stream


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$stream` | `resource` | *není* | The target stream. |
| `$filtername` | `string` | *není* | The filter name. |
| `$read_write` | `int` | null, | By default, stream_filter_append will attach the filter to the read filter chain if the file was opened for reading (i.e. File Mode: r, and/or +). The filter will also be attached to the write filter chain if the file was opened for writing (i.e. File Mode: w, a, and/or +). STREAM_FILTER_READ, STREAM_FILTER_WRITE, and/or STREAM_FILTER_ALL can also be passed to the read_write parameter to override this behavior. |
| `$params` | `mixed` | null | This filter will be added with the specified params to the end of the list and will therefore be called last during stream operations. To add a filter to the beginning of the list, use stream_filter_prepend. |


Návratové hodnoty
----------------

`resource`

a resource which can be used to refer to this filter
instance during a call to stream_filter_remove.

Další zdroje
------------

[Oficiální dokumentace funkce stream-filter-append](https://www.php.net/manual/en/function.stream-filter-append.php)
