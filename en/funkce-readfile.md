PHP function readfile()
=======================

> id: e98818bd-3b2f-4b36-983d-c3fd507d3ce3
> slug:
> 	cs: funkce-readfile
> 	en: php-function-readfile
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: f3a53f7a8fe7b586ed41e53505d6169c

Availability in `PHP 4.0`

Outputs a file


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | The filename being read. |
| `$use_include_path` | `bool` | null, | You can use the optional second parameter and set it to true, if you want to search for the file in the include_path, too. |
| `$context` | `resource` | null | A context stream resource. |


Return values
----------------

`int`

the number of bytes read from the file. If an error
occurs, false is returned and unless the function was called as

Other resources
------------

[Official readfile function documentation](https://www.php.net/manual/en/function.readfile.php)
