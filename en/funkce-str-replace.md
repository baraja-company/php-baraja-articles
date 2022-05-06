PHP function str_replace()
==========================

> id: bda3769f-76b8-4458-9c75-178979c73cad
> slug:
> 	cs: funkce-str-replace
> 	en: php-function-str-replace
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '292f5b77d0c680f1010d89c2fe5d010f'

Availability in `PHP 4.0`

Replaces all occurrences of the search string with the replacement string.

Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$search` | `mixed` | *not* | Search value, otherwise known as `$needle`. The field can be used to denote multiple `$needle`. |
| `$replace` | `mixed` | *not* | Replacement content for the found value. The field can be used for multiple replacements. |
| | `$subject` | `mixed` | *not* | String or array in which to substitute. Referred to as ``$haystack``. |
| `$count` | `int` | null | | |


Return values
----------------

`mixed`

This function returns a string or array of substituted values.

```php
echo str_replace('https://', 'https://', 'https://google.com'); // https://google.com
```

Other resources
------------

[Official str-replace documentation](https://www.php.net/manual/en/function.str-replace.php)
