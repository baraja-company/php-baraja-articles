PHP function imagecropauto()
============================

> id: '7e33e418-3e4b-4d6b-9500-540a986258d2'
> slug:
> 	cs: funkce-imagecropauto
> 	en: php-function-imagecropauto
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '76b7966cfb5d73a2242c939660760aa5'

Availability in versions: `PHP 5.5.0`

Crop an image automatically using one of the available modes


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$image` | `resource` | | An image resource, returned by one of the image creation functions, such as {@link https://www.php.net/manual/en/function.imagecreatetruecolor.php imagecreatetruecolor()}. |
| `$mode` | `int` | -1, | One of <b>IMG_CROP_*</b> constants. |
| `$threshold` | `float` | .5, | Used <b>IMG_CROP_THRESHOLD</b> mode. |
| `$color` | `int` | -1 | |


Return values
----------------

`resource`

|bool Return cropped image resource on success or <b>FALSE</b> on failure.

Other resources
------------


- https://www.php.net/manual/en/function.imagecropauto.php
- https://www.php.net/manual/en/function.imagecreatetruecolor.php imagecreatetruecolor()}.
</p>
