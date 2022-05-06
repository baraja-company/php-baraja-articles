PHP function getmxrr()
======================

> id: '95f29209-27d7-4514-9488-30e26b3fdabc'
> slug:
> 	cs: funkce-getmxrr
> 	en: php-function-getmxrr
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '4249c67d71ca9ef330f2f938fa211283'

Availability in `PHP 4.0`

Get MX records corresponding to a given Internet host name


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$hostname` | `string` | *not* | The Internet host name. |
| `$mxhosts` | `array` | *not* | A list of the MX records found is placed into the array mxhosts. |
| | `$weight` | `array` | null | If the weight array is given, it will be filled with the weight information gathered. |


Return values
----------------

`bool`

true if any records are found; returns false if no records
were found or if an error occurred.

Other resources
------------

[Official getmxrr documentation](https://www.php.net/manual/en/function.getmxrr.php)
