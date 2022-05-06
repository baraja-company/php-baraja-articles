PHP function socket_set_timeout()
=================================

> id: '3afe8aaf-5ad8-4929-b0ef-ab8ac6d66328'
> slug:
> 	cs: funkce-socket-set-timeout
> 	en: php-function-socket-set-timeout
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: f805d3c1a33c7540991cfe2cc6efe207

Availability in `PHP 4.0`

&Alias; <function>stream_set_timeout</function>
<p>Set timeout period on a stream


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$stream` | `resource` | *not* | The target stream. |
| `$seconds` | `int` | *not* | The seconds part of the timeout to be set. |
| | `$microseconds` | `int` | 0 | The microseconds part of the timeout to be set. |


Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.

Other resources
------------

[Official socket-set-timeout documentation](https://www.php.net/manual/en/function.socket-set-timeout.php)
