PHP function uniqid()
=====================

> id: '6871ccc8-590b-4cc9-ba18-d2aeab2b0838'
> slug:
> 	cs: funkce-uniqid
> 	en: php-function-uniqid
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '655419a65ff128aaa3fd1751c45934d4'

Availability in `PHP 4.0`

Generates a unique identifier.

It is mainly used for generating IDs in tables that are stored piecemeal on multiple servers and are periodically synchronized. If the database is running in multiple places at the same time, the uniqueness of each numeric ID cannot be guaranteed as an autoincrement (because there would have to be a central server that allocates them), so a UID is used, a string that has a different prefix on each machine and only its internal structure is generated, but the base remains the same. Thus, there is no collision of identifiers during subsequent synchronization.

The generated identifier contains hyphens and other characters. If we need to guarantee the same length of consecutive letters and digits, we can hash the identifier: `md5(uniqid())`.

Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$prefix` | `string` | `""` | May be useful, for example, if you are generating identifiers simultaneously on multiple servers that might generate an identifier in the same microsecond. |
| `$more_entropy` | `bool` | `false` | When set to `true`, uniqid adds additional entropy at the end of the return value (using a combined linear congruence generator), which should make the results more unique. |


Return values
----------------

`string`

Unique identifier as a string.

Other resources
------------

[Official uniqid documentation](https://www.php.net/manual/en/function.uniqid.php)
