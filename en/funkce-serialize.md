PHP function serialize()
========================

> id: bfb18be6-3d30-432f-a55e-66f1f771fcef
> slug:
> 	cs: funkce-serialize
> 	en: php-function-serialize
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: a5c35ade72fac0cc6c2d0575c9b476ce

Availability in `PHP 4.0`

Generates a storable representation of a value


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$value` | `mixed` | *not* | The value to be serialized. serialize handles all types, except the resource-type. You can even serialize arrays that contain references to itself. Circular references inside the array/object you are serializing will also be stored. Any other reference will be lost. |


Return values
----------------

`string`

a string containing a byte-stream representation of
value that can be stored anywhere.

Other resources
------------

[Official serialize documentation](https://www.php.net/manual/en/function.serialize.php)
