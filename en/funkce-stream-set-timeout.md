PHP function stream_set_timeout()
=================================

> id: a6163d2c-53e7-43e6-b10c-962f09f52095
> slug:
> 	cs: funkce-stream-set-timeout
> 	en: php-function-stream-set-timeout
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '38dd5dbf2775ccb2e4c88d273a67a418'

Availability in versions: `PHP 4.3.0`

Set timeout period on a stream


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$stream` | `resource` | *not* | The target stream. |
| `$seconds` | `int` | *not* | The seconds part of the timeout to be set. |
| | `$microseconds` | `int` | null | The microseconds part of the timeout to be set. |


Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.

Other resources
------------

[Official stream-set-timeout documentation](https://www.php.net/manual/en/function.stream-set-timeout.php)
