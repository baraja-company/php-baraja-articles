PHP function stream_filter_append()
===================================

> id: '884e9069-ed27-407c-8320-e0aed9a01466'
> slug:
> 	cs: funkce-stream-filter-append
> 	en: php-function-stream-filter-append
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '4a84befa40a22f048b6704f6d172d823'

Availability in versions: `PHP 4.3.0`

Attach a filter to a stream


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$stream` | `resource` | *not* | The target stream. |
| `$filtername` | `string` | *not* | The filter name. |
| `$read_write` | `int` | null, | By default, stream_filter_append will attach the filter to the read filter chain if the file was opened for reading (i.e. File Mode: r, and/or +). The filter will also be attached to the write filter chain if the file was opened for writing (i.e. File Mode: w, a, and/or +). STREAM_FILTER_READ, STREAM_FILTER_WRITE, and/or STREAM_FILTER_ALL can also be passed to the read_write parameter to override this behavior. |
| `$params` | `mixed` | null | This filter will be added with the specified params to the end of the list and will therefore be called last during stream operations. To add a filter to the beginning of the list, use stream_filter_prepend. |


Return values
----------------

`resource`

a resource which can be used to refer to this filter
instance during a call to stream_filter_remove.

Other resources
------------

[Official stream-filter-append documentation](https://www.php.net/manual/en/function.stream-filter-append.php)
