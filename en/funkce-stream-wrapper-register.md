PHP function stream_wrapper_register()
======================================

> id: '97f1df5d-d5d6-4cb9-83c2-3b85e7247597'
> slug:
> 	cs: funkce-stream-wrapper-register
> 	en: php-function-stream-wrapper-register
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '07d583b9f1c3b488f152270dc31467c9'

Availability in versions: `PHP 4.3.2`

Register a URL wrapper implemented as a PHP class


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$protocol` | `string` | *not* | The wrapper name to be registered. |
| `$classname` | `string` | *not* | The classname which implements the protocol. |
| `$flags` | `int` | null | Should be set to STREAM_IS_URL if protocol is a URL protocol. Default is 0, local stream. |


Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.
</p>
<p>
stream_wrapper_register will return false if the
protocol already has a handler.

Additional resources
------------

[Official stream-wrapper-register documentation](https://www.php.net/manual/en/function.stream-wrapper-register.php)
