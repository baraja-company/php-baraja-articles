PHP function filectime()
========================

> id: f0794d26-873a-4b57-99f5-992af7e32ac7
> slug:
> 	cs: funkce-filectime
> 	en: php-function-filectime
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '25463d10f1bdc4e069425ccb9ea260ea'

Availability in `PHP 4.0`

Gets inode change time of file


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | Path to the file. |


Return values
----------------

`int`

|bool the time the file was last changed, or false on failure.
The time is returned as a Unix timestamp.

Other resources
------------

[Official filectime documentation](https://www.php.net/manual/en/function.filectime.php)
