PHP funkce imagesetinterpolation()
==================================

> id: "67682117-61eb-4ed2-8315-491f526b2621"
> slug:
> 	cs: funkce-imagesetinterpolation
> 
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.5.0`

Set the interpolation method


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$image` | `resource` |  | An image resource, returned by one of the image creation functions, such as {@link https://www.php.net/manual/en/function.imagecreatetruecolor.php imagecreatetruecolor()}. |
| `$method` | `int` | IMG_BILINEAR_FIXED | The interpolation method, which can be one of the following: <ul> <li> IMG_BELL: Bell filter. </li> <li> IMG_BESSEL: Bessel filter. </li> <li> IMG_BICUBIC: Bicubic interpolation. </li> <li> IMG_BICUBIC_FIXED: Fixed point implementation of the bicubic interpolation. </li> <li> IMG_BILINEAR_FIXED: Fixed point implementation of the bilinear interpolation (<em>default (also on image creation)</em>). </li> <li> IMG_BLACKMAN: Blackman window function. </li> <li> IMG_BOX: Box blur filter. </li> <li> IMG_BSPLINE: Spline interpolation. </li> <li> IMG_CATMULLROM: Cubbic Hermite spline interpolation. </li> <li> IMG_GAUSSIAN: Gaussian function. </li> <li> IMG_GENERALIZED_CUBIC: Generalized cubic spline fractal interpolation. </li> <li> IMG_HERMITE: Hermite interpolation. </li> <li> IMG_HAMMING: Hamming filter. </li> <li> IMG_HANNING: Hanning filter. </li> <li> IMG_MITCHELL: Mitchell filter. </li> <li> IMG_POWER: Power interpolation. </li> <li> IMG_QUADRATIC: Inverse quadratic interpolation. </li> <li> IMG_SINC: Sinc function. </li> <li> IMG_NEAREST_NEIGHBOUR: Nearest neighbour interpolation. </li> <li> IMG_WEIGHTED4: Weighting filter. </li> <li> IMG_TRIANGLE: Triangle interpolation. </li> </ul> |


Návratové hodnoty
----------------

`bool`

Returns TRUE on success or FALSE on failure.

Další zdroje
------------


- https://www.php.net/manual/en/function.imagesetinterpolation.php
- https://www.php.net/manual/en/function.imagecreatetruecolor.php imagecreatetruecolor()}.
</p>
