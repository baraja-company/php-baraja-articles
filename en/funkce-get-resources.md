PHP function get_resources()
============================

> id: a3b44e91-1d53-40e5-84cb-8815dc58b2f9
> slug:
> 	cs: funkce-get-resources
> 	en: php-function-get-resources
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: b3d5c1faf3779823e5ec91b426e563d6

Availability in `PHP 7.0`

Returns active resources


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$type` | `string` | *not* | If defined, this will cause get_resources() to only return resources of the given type. A list of resource types is available. If the string Unknown is provided as the type, then only resources that are of an unknown type will be returned. If omitted, all resources will be returned. |


Return values
----------------

`array`

Returns an array of currently active resources, indexed by resource number.

Other resources
------------

[Official get-resources documentation](https://www.php.net/manual/en/function.get-resources.php)
