PHP function ob_get_status()
============================

> id: '2eb99e32-7be9-49e2-a306-4c9c314bb7c4'
> slug:
> 	cs: funkce-ob-get-status
> 	en: php-function-ob-get-status
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '2d182a2cc04f5a672c2164c9018f6d8e'

Availability in versions: `PHP 4.2.0`

Get status of output buffers


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$full_status` | `bool` | null | true to return all active output buffer levels. If false or not set, only the top level output buffer is returned. |


Return values
----------------

`array`

If called without the full_status parameter
or with full_status = false a simple array
with the following elements is returned:
2
[type] => 0
[status] => 0
[name] => URL-Rewriter
[del] => 1
)
]]>
Simple ob_get_status results
KeyValue
levelOutput nesting level
typePHP_OUTPUT_HANDLER_INTERNAL (0) or PHP_OUTPUT_HANDLER_USER (1)
statusOne of PHP_OUTPUT_HANDLER_START (0), PHP_OUTPUT_HANDLER_CONT (1) or PHP_OUTPUT_HANDLER_END (2)
nameName of active output handler or ' default output handler' if none is set
delErase-flag as set by ob_start
</p>
<p>
If called with full_status = true an array
with one element for each active output buffer level is returned.
The output level is used as key of the top level array and each array
element itself is another array holding status information
on one active output level.
Array
(
[chunk_size] => 0
[size] => 40960
[block_size] => 10240
[type] => 1
[status] => 0
[name] => default output handler
[del] => 1
)
[1] => Array
(
[chunk_size] => 0
[size] => 40960
[block_size] => 10240
[type] => 0
[buffer_size] => 0
[status] => 0
[name] => URL-Rewriter
[del] => 1
)
)
]]>
</p>
<p>
The full output contains these additional elements:
Full ob_get_status results
KeyValue
chunk_sizeChunk size as set by ob_start
size...
blocksize...

Additional resources
------------

[Official ob-get-status documentation](https://www.php.net/manual/en/function.ob-get-status.php)
