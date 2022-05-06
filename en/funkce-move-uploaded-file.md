PHP function move_uploaded_file()
=================================

> id: aa358b2e-0e8e-4110-9677-50fb7d4a3e30
> slug:
> 	cs: funkce-move-uploaded-file
> 	en: php-function-move-uploaded-file
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '8c82c2b4a611e54781d79308fc9dd21b'

Availability in versions: `PHP 4.0.3`

Moves an uploaded file to a new location


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | The filename of the uploaded file. |
| `$destination` | `string` | *not* | The destination of the moved file. |


Return values
----------------

`bool`

If filename is not a valid upload file,
then no action will occur, and
move_uploaded_file will return
false.
</p>
<p>
If filename is a valid upload file, but
cannot be moved for some reason, no action will occur, and
move_uploaded_file will return
false. Additionally, a warning will be issued.

Other resources
------------

[Official move-uploaded-file documentation](https://www.php.net/manual/en/function.move-uploaded-file.php)
