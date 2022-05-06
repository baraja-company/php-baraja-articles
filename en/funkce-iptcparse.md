PHP function iptcparse()
========================

> id: a768ebe8-827c-40cf-b56e-f5661e665091
> slug:
> 	cs: funkce-iptcparse
> 	en: php-function-iptcparse
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: c81997ee56e2aea720591255604206be

Availability in `PHP 4.0`

Parse a binary IPTC block into single tags.


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$iptcblock` | `string` | *not* | A binary IPTC block. |


Return values
----------------

`array`

an array using the tagmarker as an index and the value as the
value. It returns false on error or if no IPTC data was found.

Other resources
------------

[Official iptcparse documentation](https://www.php.net/manual/en/function.iptcparse.php)
