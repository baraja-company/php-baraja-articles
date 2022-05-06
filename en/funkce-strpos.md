PHP function strpos - occurrence of a substring in a string
===========================================================

> id: '4f7386ff-3ff4-4d62-ac65-97c78ef605e0'
> slug:
> 	cs: funkce-strpos
> 	en: php-function-strpos---occurrence-of-a-substring-in-a-string
> 
> publicationDate: '2019-11-26 11:53:30'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '15f9b69aef3a76cb3fe9e3a0c70895e2'

The function finds the position of the first occurrence of the substring in the string, which in human terms means that it checks if the passed string contains the search expression and returns its position.

The `strpos` function returns the position of the search text in the string. If the string contains it, it returns the position of its first character as `integer` (integer), if it does not, it returns `false` (false). This can be used when testing strings.

Parameters
---------

| Parameter | Data type | Default value | Note |
|-------------|------------|-----------------|-----|
| `$haystack` | `string` | *required* | String to be searched |
| `$needle` | `mixed` | *required* | If not a string, it is converted to a number (integer) and used as the ordinal value of the character. |
| `$offset` | `int` | `0` | If a value is set, the function will first subtract the specified number of characters from the beginning of the string before starting the search. Unlike the `strrpos` and `strripos` functions, the offset cannot be negative. |

String occurrence in text - PHP 8 update
------------------------------------------

As of PHP 8, there is a native way to verify the existence of a string in text. This is done very simply with the native `str_contains` function:

```php
if (str_contains('jan@barasek.com', '@')) {
    // The email contains an alias
}
```

String occurrence in text
----------------------

> **Warning:** The `str_contains` function has been a native part of the language since PHP 8.

We often need to find out if a text contains a certain string. Until PHP 8, the `str_contains` function did not exist, so it was previously implemented as follows:

```php
/**
 * Does $haystack contain the string $needle?
 */
function str_contains(string $haystack, string $needle): bool
{
    return strpos($haystack, $needle) !== false;
}
```

For example, you can construct a condition to see if a string contains a second string and then proceed accordingly:

```php
if (strpos('jan@barasek.com', '@') !== false) {
    // The email contains an alias
}
```

The advantage of this method of string validation is its extreme speed. If you need to verify a more complex match, <a href="/regex">regular expressions</a> will be needed.

Return values
----------------

The function always returns the string position as an integer (`int`), or the value `false` if the string does not contain the search phrase.

Additional resources
------------

[Official strpos function documentation]([Official manual](https://www.php.net/manual/en/function.strpos.php))
