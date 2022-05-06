PHP function trim()
===================

> id: a8b76833-94a0-4757-bdef-b480e0aa8d03
> slug:
> 	cs: funkce-trim
> 	en: php-function-trim
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '11fc0c7affe2cf78f8bb4837ccf8b257'

Availability in `PHP 4.0`

Deletes whitespace (or other characters) from the beginning and end of a string.

Alternatively, use `ltrim()` (deletes from the left), or `rtrim()` (deletes from the right).

Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$str` | `string` | *not* | String to be truncated. |
| `$charlist` | `string` | " | Optionally, trimmed characters can also be specified using the `$charlist` parameter. Simply specify all the characters you want to get rid of. Use the `..` construct to specify a range of characters. |

Return values
----------------

`string`

Modified string

```php
echo trim(' Hi, how are you? '); // "Hi, how are you?"
```

Other resources
------------

[Official documentation of the trim function](https://www.php.net/manual/en/function.trim.php)
