PHP function fputcsv()
======================

> id: c718ddd4-d5ce-4c3e-bbd8-2f4989d19b00
> slug:
> 	cs: funkce-fputcsv
> 	en: php-function-fputcsv
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '8617bc1b205057974338cfef7bbf0cbe'

Availability in versions: `PHP 5.1.0`

Format line as CSV and write to file pointer


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$handle` | `resource` | *not* | |
| `$fields` | `array` | *not* | An array of values. |
| `$delimiter` | `string` | ",", | The optional delimiter parameter sets the field delimiter (one character only). |
| `$enclosure` | `string` | '"', | The optional enclosure parameter sets the field enclosure (one character only). |
| | `$escape_char` | `string` | "\\" | |


Return values
----------------

`int`

|bool the length of the written string or false on failure.

Other resources
------------

[Official fputcsv documentation](https://www.php.net/manual/en/function.fputcsv.php)
