PHP function file_exists()
==========================

> id: '49446f2d-4091-420e-b5d2-3023c2861fe0'
> slug:
> 	cs: funkce-file-exists
> 	en: php-function-file-exists
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: f60333932197f39e71fcdb374d352c98

Availability in `PHP 4.0`

Checks whether a file or directory exists


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | Path to the file or directory. |


Return values
----------------

`bool`

true if the file or directory specified by
filename exists; false otherwise.
</p>
<p>
This function will return false for symlinks pointing to non-existing
files.
</p>
<p>
This function returns false for files inaccessible due to safe mode restrictions. However these
files still can be included if
they are located in safe_mode_include_dir.
</p>
<p>
The check is done using the real UID/GID instead of the effective one.

Other resources
------------

[Official file-exists documentation](https://www.php.net/manual/en/function.file-exists.php)
