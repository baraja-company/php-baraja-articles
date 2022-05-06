PHP function stream_register_wrapper()
======================================

> id: c05704b9-26dd-4918-acb7-520519c8b3e8
> slug:
> 	cs: funkce-stream-register-wrapper
> 	en: php-function-stream-register-wrapper
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '572d8dc2d5c185095a8409a1012f5dba'

Availability in versions: `PHP 4.3.0`

&Alias; <function>stream_wrapper_register</function>
<p>Register a URL wrapper implemented as a PHP class


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$protocol` | `string` | *not* | The wrapper name to be registered. |
| `$classname` | `string` | *not* | The classname which implements the protocol. |
| `$flags` | `int` | *not* | Should be set to STREAM_IS_URL if protocol is a URL protocol. Default is 0, local stream. |


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

[Official stream-register-wrapper documentation](https://www.php.net/manual/en/function.stream-register-wrapper.php)
