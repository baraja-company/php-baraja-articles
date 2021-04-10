PHP funkce getimagesizefromstring()
===================================

> id: b5c567bd-5bcf-4ccd-baae-fa5994f7b2df
> slug:
> 	cs: funkce-getimagesizefromstring
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

PHP > 5.4.0<br/>
Get the size of an image from a string.


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$imagedata` | `string` |  |  |
| `$imageinfo` | `array` | null |  |


Návratové hodnoty
----------------

`array`

|bool Returns an array with 7 elements.<br>
Index 0 and 1 contains respectively the width and the height of the image.<br>
Index 2 is one of the <b>IMAGETYPE_XXX</b> constants indicating the type of the image.<br>
Index 3 is a text string with the correct <b>height="yyy" width="xxx"</b> string<br>
that can be used directly in an IMG tag.<br>
On failure, FALSE is returned.

Další zdroje
------------

https://www.php.net/manual/en/function.getimagesizefromstring.php
