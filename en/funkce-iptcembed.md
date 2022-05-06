PHP function iptcembed()
========================

> id: '809d2b67-f45d-4555-a074-c5dfa6873eca'
> slug:
> 	cs: funkce-iptcembed
> 	en: php-function-iptcembed
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: ae5a19a0a77797ad5774f3a1dcf30bf5

Availability in `PHP 4.0`

Embeds binary IPTC data into a JPEG image


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$iptcdata` | `string` | *not* | The data to be written. |
| `$jpeg_file_name` | `string` | *not* | Path to the JPEG image. |
| `$spool` | `int` | null | Spool flag. If the spool flag is over 2 then the JPEG will be returned as a string. |


Return values
----------------

`mixed`

If success and spool flag is lower than 2 then the JPEG will not be
returned as a string, false on errors.

Other resources
------------

[Official iptcembed documentation](https://www.php.net/manual/en/function.iptcembed.php)
