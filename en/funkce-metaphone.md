PHP function metaphone()
========================

> id: '95b5f0a7-0140-41bf-ab46-b5a712207209'
> slug:
> 	cs: funkce-metaphone
> 	en: php-function-metaphone
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: faad9141f99a86a4d76db383f2f8dc61

Availability in `PHP 4.0`

Calculate the metaphone key of a string


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$str` | `string` | *not* | String being processed. |
| `$phonemes` | `int` | 0 | This parameter restricts the returned metaphone key to phonemes characters in length. The default value of 0 means no restriction. |


Return values
----------------

`string`

|bool the metaphone key as a string, or FALSE on failure

Other resources
------------

[Official metaphone documentation](https://www.php.net/manual/en/function.metaphone.php)
