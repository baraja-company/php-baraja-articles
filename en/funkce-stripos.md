PHP function stripos()
======================

> id: '93bcf683-be6f-4cfa-84ee-69b04af69604'
> slug:
> 	cs: funkce-stripos
> 	en: php-function-stripos
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '537c382b24fc953f7b6ae59273e2d110'

Availability in `PHP 5.0`

Finds the position of the first occurrence of a string without character size resolution.

> **TIP:**
>
> The `stripos` function has been used in the past to check whether a string contains a substring.
> Since PHP 8.0, there is a native `str_contains()` function for this.

Parameters
--------------

| Parameter | Data Type | Default Value | Note |
|-----|-----|-----|-----|
| `$haystack` | `string` | *not* | String to search |
| `$needle` | `string` | *not* | Note that a needle can be a string of one or more characters. |
| `$offset` | `int` | null | The optional offset parameter allows you to specify which character in the haystack to start searching for. The returned position is still relative to the start of the stack. |


Return values
----------------

`int` - returns the position where the found substring starts.

If the substring is not found in the searched string, return `false`.

Other resources
------------

[Official stripos documentation](https://www.php.net/manual/en/function.stripos.php)
