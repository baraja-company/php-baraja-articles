PHP function stream_context_create()
====================================

> id: '813becd1-4e37-4294-8481-c40d23cf40fb'
> slug:
> 	cs: funkce-stream-context-create
> 	en: php-function-stream-context-create
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: c2441d78f7b9e22d4afd790d14ed96e3

Availability in versions: `PHP 4.3.0`

Create a streams context


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$options` | `array` | null, | Must be an associative array of associative arrays in the format $arr['wrapper']['option'] = $value. |
| `$params` | `array` | null | Must be an associative array in the format $arr['parameter'] = $value. Refer to context parameters for a listing of standard stream parameters. |


Return values
----------------

`resource`

A stream context resource.

Other resources
------------

[Official documentation for stream-context-create](https://www.php.net/manual/en/function.stream-context-create.php)
