PHP function header()
=====================

> id: cf7f8ad4-5038-4519-bd1c-712028a494a5
> slug:
> 	cs: funkce-header
> 	en: php-function-header
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: b9ef7236dc4e7f4edf6e177f874c7adb

Availability in `PHP 4.0`

Sends the HTTP header and the raw form.

Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$string` | `string` | *not* | Header content |
| `$replace` | `bool` | true, | The optional replace parameter indicates whether the header should replace a previous similar header, or add a second header of the same type. By default it will replace, but if you pass in false as the second argument you can force multiple headers of the same type. For example: |
| `$http_response_code` | `int` | null | Forces the HTTP response code to the specified value. |


Return values
----------------

Returns nothing.


Other Resources
------------

[Official header documentation](https://www.php.net/manual/en/function.header.php)
