PHP function imagecrop()
========================

> id: a7d425ff-567c-4e71-b019-23b818a68320
> slug:
> 	cs: funkce-imagecrop
> 	en: php-function-imagecrop
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: d75c12cdd5e9da3c9c502142ebd9c023

Availability in versions: `PHP 5.5.0`

Crop an image using the given coordinates and size, x, y, width and height


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$image` | `resource` | | An image resource, returned by one of the image creation functions, such as {@link https://www.php.net/manual/en/function.imagecreatetruecolor.php imagecreatetruecolor()}. |
| `$rect` | `array` | | Array with keys "x", "y", "width" and "height". |


Return values
----------------

`resource`

|bool Return cropped image resource on success or FALSE on failure.

Other resources
------------


- https://www.php.net/manual/en/function.imagecrop.php
- https://www.php.net/manual/en/function.imagecreatetruecolor.php imagecreatetruecolor()}.
</p>
