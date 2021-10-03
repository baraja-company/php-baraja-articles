PHP funkce getimagesize - velikost obrázku
==========================================

> id: "9cfd63c7-a928-4640-9f13-5deccf73c7f2"
> slug:
> 	cs: funkce-getimagesize
>
> publicationDate: "2019-11-26 11:56:18"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Funkce vrátí fyzické rozměry obrázku.

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` |  | This parameter specifies the file you wish to retrieve information about. It can reference a local file or (configuration permitting) a remote file using one of the supported streams. |
| `$imageinfo` | `array` | null | This optional parameter allows you to extract some extended information from the image file. Currently, this will return the different JPG APP markers as an associative array. Some programs use these APP markers to embed text information in images. A very common one is to embed IPTC information in the APP13 marker. You can use the iptcparse function to parse the binary APP13 marker into something readable. |


Návratové hodnoty
----------------

`array`

|bool an array with 7 elements.
</p>
<p>
Index 0 and 1 contains respectively the width and the height of the image.
</p>
<p>
Some formats may contain no image or may contain multiple images. In these
cases, getimagesize might not be able to properly
determine the image size. getimagesize will return
zero for width and height in these cases.
</p>
<p>
Index 2 is one of the IMAGETYPE_XXX constants indicating
the type of the image.
</p>
<p>
Index 3 is a text string with the correct
height="yyy" width="xxx" string that can be used
directly in an IMG tag.
</p>
<p>
mime is the correspondant MIME type of the image.
This information can be used to deliver images with correct the HTTP
Content-type header:
getimagesize and MIME types
]]>
</p>
<p>
channels will be 3 for RGB pictures and 4 for CMYK
pictures.
</p>
<p>
bits is the number of bits for each color.
</p>
<p>
For some image types, the presence of channels and
bits values can be a bit
confusing. As an example, GIF always uses 3 channels
per pixel, but the number of bits per pixel cannot be calculated for an
animated GIF with a global color table.
</p>
<p>
On failure, false is returned.

Další zdroje
------------

https://www.php.net/manual/en/function.getimagesize.php
