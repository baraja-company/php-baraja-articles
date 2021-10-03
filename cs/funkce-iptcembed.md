PHP funkce iptcembed()
======================

> id: "809d2b67-f45d-4555-a074-c5dfa6873eca"
> slug:
> 	cs: funkce-iptcembed
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Embeds binary IPTC data into a JPEG image


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$iptcdata` | `string` |  | The data to be written. |
| `$jpeg_file_name` | `string` |  | Path to the JPEG image. |
| `$spool` | `int` | null | Spool flag. If the spool flag is over 2 then the JPEG will be returned as a string. |


Návratové hodnoty
----------------

`mixed`

If success and spool flag is lower than 2 then the JPEG will not be
returned as a string, false on errors.

Další zdroje
------------

https://www.php.net/manual/en/function.iptcembed.php
