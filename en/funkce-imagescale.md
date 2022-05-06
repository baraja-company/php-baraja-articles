PHP function imagescale()
=========================

> id: '57134d08-36c4-417b-90ab-74becd2aa754'
> slug:
> 	cs: funkce-imagescale
> 	en: php-function-imagescale
> 
> publicationDate: '2020-02-16 16:23:15'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '145814eef7ebdc6c28cbffea3e9bb281'

Availability in versions: `PHP 5.5.0`

Rescales (resizes) the image to the given width and height.

Parameters
--------------

| Parameter | Data type | Default value | Note |
|---------------|------------|--------|-----|
| `$image` | `resource` | *not* | The source of the image (data type `resource`) that you get by one of the functions to create or load an image, such as [imagecreatetruecolor()](https://www.php.net/manual/en/function.imagecreatetruecolor.php). |
| `$new_width` | `int` | *not* | New width. |
| `$new_height` | `int` | `-1` | New height. |
| | `$mode` | `int` | `IMG_BILINEAR_FIXED` | Image editing mode. |


Return values
----------------

`resource` or `false`.

The function returns the modified image as a `resource` data type. If an error occurs, it returns `false`.

Other resources
------------

[Official imagescale function documentation](- [imagescale() function](https://www.php.net/manual/en/function.imagescale.php))
- [imagecreatetruecolor() function](https://www.php.net/manual/en/function.imagecreatetruecolor.php)
