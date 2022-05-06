PHP function implode()
======================

> id: e69cb651-79da-43ec-81fb-11afa2a19c77
> slug:
> 	cs: funkce-implode
> 	en: php-function-implode
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: b15a706e783c5738a9b61ac2a21ffe09

Availability in `PHP 4.0`

Join array elements with a string


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$glue` | `string` | "", | Defaults to an empty string. This is not the preferred usage of implode as glue would be the second parameter and thus, the bad prototype would be used. |
| `$pieces` | `array` | *not* | The array of strings to implode. |


Return values
----------------

`string`

a string containing a string representation of all the array
elements in the same order, with the glue string between each element.

Other resources
------------

[Official implode documentation](https://www.php.net/manual/en/function.implode.php)
