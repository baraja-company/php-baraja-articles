PHP function filetype()
=======================

> id: c11a3af6-ef35-44de-b171-f22eb58ed6d1
> slug:
> 	cs: funkce-filetype
> 	en: php-function-filetype
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '6643b8ff02f8f4a5ddd4d8a5012457fa'

Availability in `PHP 4.0`

Gets file type


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | Path to the file. |


Return values
----------------

`string`

the type of the file. Possible values are fifo, char,
dir, block, link, file, socket and unknown.
</p>
<p>
Returns false if an error occurs. filetype will also
produce an E_NOTICE message if the stat call fails
or if the file type is unknown.

Other resources
------------

[Official filetype documentation](https://www.php.net/manual/en/function.filetype.php)
