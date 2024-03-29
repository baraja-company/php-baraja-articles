PHP function error_log()
========================

> id: f8454528-e7d2-4784-8a40-ba6249988167
> slug:
> 	cs: funkce-error-log
> 	en: php-function-error-log
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '8b4e1313e8d1f52306c86406df0b84ac'

Availability in `PHP 4.0`

Send an error message somewhere


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$message` | `string` | *not* | The error message that should be logged. |
| `$message_type` | `int` | null, | Says where the error should go. The possible message types are as follows: |
| `$destination` | `string` | null, | The destination. Its meaning depends on the message_type parameter as described above. |
| `$extra_headers` | `string` | null | The extra headers. It's used when the message_type parameter is set to 1. This message type uses the same internal function as mail does. |


Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.

Other resources
------------

[Official documentation for the error-log function](https://www.php.net/manual/en/function.error-log.php)
