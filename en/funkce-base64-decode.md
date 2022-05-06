PHP function base64_decode()
============================

> id: a587925c-9696-42d8-93e0-e9db64747a6b
> slug:
> 	cs: funkce-base64-decode
> 	en: php-function-base64-decode
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '48b3712a491164e792c4b4980be76e72'

Availability in `PHP 4.0`

Decodes data encoded with MIME base64


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$data` | `string` | *not* | The encoded data. |
| `$strict` | `bool` | null | Returns false if input contains character from outside the base64 alphabet. |


Return values
----------------

`string`

|bool the original data or false on failure. The returned data may be
binary.

Other resources
------------

[Official documentation of the base64-decode function](https://www.php.net/manual/en/function.base64-decode.php)
