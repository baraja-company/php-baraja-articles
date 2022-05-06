PHP function wordwrap()
=======================

> id: '2f2222d8-193f-422f-8d46-9f1c485abb12'
> slug:
> 	cs: funkce-wordwrap
> 	en: php-function-wordwrap
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '166e5f8389bc524d27862a11b73e9201'

Availability in versions: `PHP 4.0.2`

Wraps a string to the specified number of characters


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$str` | `string` | *not* | String being processed. |
| `$width` | `int` | 75, | column width. |
| `$break` | `string` | "\n", | The string is broken using the optional break parameter. |
| `$cut` | `bool` | false | If the cut parameter is set to true, the string will always wrap at or before the specified width. Thus, if the word is larger than the specified width, it is split. (See the second example). |


Return values
----------------

`string`

The given string wrapped in the specified column.

Other resources
------------

[Official wordwrap documentation](https://www.php.net/manual/en/function.wordwrap.php)
