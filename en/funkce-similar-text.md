PHP function similar_text()
===========================

> id: '9eed8734-b132-4203-9ccb-c09889d0c023'
> slug:
> 	cs: funkce-similar-text
> 	en: php-function-similar-text
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '53baabc34181fdaf87423000ed71cae1'

Availability in `PHP 4.0`

Calculate the similarity between two strings


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$first` | `string` | *not* | The first string. |
| `$second` | `string` | *not* | The second string. |
| | `$percent` | `float` | null | By passing a reference as third argument, similar_text will calculate the similarity in percent for you. |


Return values
----------------

`int`

the number of matching chars in both strings.

Other resources
------------

[Official similar-text documentation](https://www.php.net/manual/en/function.similar-text.php)
