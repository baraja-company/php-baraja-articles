PHP funkce image_type_to_mime_type()
================================

> id: c02c40df-b44d-43cb-b5ed-9e3350d8bd27
> slugCS: funkce-image-type-to-mime-type
> publicationDate: 2019-09-11 10:04:03
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 4.3.0`

Get Mime-Type for image-type returned by getimagesize,


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$imagetype` | `int` |  | One of the IMAGETYPE_XXX constants. |


Návratové hodnoty
----------------

`string`

The returned values are as follows
<table>
Returned values Constants
<tr valign="top">
<td>imagetype</td>
<td>Returned value</td>
</tr>
<tr valign="top">
<td>IMAGETYPE_GIF</td>
<td>image/gif</td>
</tr>
<tr valign="top">
<td>IMAGETYPE_JPEG</td>
<td>image/jpeg</td>
</tr>
<tr valign="top">
<td>IMAGETYPE_PNG</td>
<td>image/png</td>
</tr>
<tr valign="top">
<td>IMAGETYPE_SWF</td>
<td>application/x-shockwave-flash</td>
</tr>
<tr valign="top">
<td>IMAGETYPE_PSD</td>
<td>image/psd</td>
</tr>
<tr valign="top">
<td>IMAGETYPE_BMP</td>
<td>image/bmp</td>
</tr>
<tr valign="top">
<td>IMAGETYPE_TIFF_II (intel byte order)</td>
<td>image/tiff</td>
</tr>
<tr valign="top">
<td>
IMAGETYPE_TIFF_MM (motorola byte order)
</td>
<td>image/tiff</td>
</tr>
<tr valign="top">
<td>IMAGETYPE_JPC</td>
<td>application/octet-stream</td>
</tr>
<tr valign="top">
<td>IMAGETYPE_JP2</td>
<td>image/jp2</td>
</tr>
<tr valign="top">
<td>IMAGETYPE_JPX</td>
<td>application/octet-stream</td>
</tr>
<tr valign="top">
<td>IMAGETYPE_JB2</td>
<td>application/octet-stream</td>
</tr>
<tr valign="top">
<td>IMAGETYPE_SWC</td>
<td>application/x-shockwave-flash</td>
</tr>
<tr valign="top">
<td>IMAGETYPE_IFF</td>
<td>image/iff</td>
</tr>
<tr valign="top">
<td>IMAGETYPE_WBMP</td>
<td>image/vnd.wap.wbmp</td>
</tr>
<tr valign="top">
<td>IMAGETYPE_XBM</td>
<td>image/xbm</td>
</tr>
<tr valign="top">
<td>IMAGETYPE_ICO</td>
<td>image/vnd.microsoft.icon</td>
</tr>
</table>

Další zdroje
------------

https://php.net/manual/en/function.image-type-to-mime-type.php
