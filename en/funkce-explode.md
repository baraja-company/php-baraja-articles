PHP function explode()
======================

> id: a1ea6a22-7673-4cac-8624-1aa48194bf8b
> slug:
> 	cs: funkce-explode
> 	en: php-function-explode
> 
> perex: Rozdělení řetězce na více částí podle oddělovače.
> publicationDate: '2019-09-11 10:59:21'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '1fd799f681636dd490c9ba2735cf1a67'

Availability in `PHP 4.0`

Parses a string according to the string type separator. Cannot set multiple delimiters.

Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$delimiter` | `string` | *not* | Separator string |
| `$string` | `string` | *not* | Input string |
| `$limit` | `int` | null | When the limit is a positive number, the returned array will contain at most as many elements as the limit. The last element of the array will possibly contain additional unparsed values that did not fit into the array. If the limit is negative, it is parsed from the end of the string. |


Return values
----------------

`array`

Returns an array of elements obtained by the separator.

If the separator is empty, it returns `false`.

If the delimiter contains a value that is not contained in the string and a negative limit is used, an empty array will be returned. For other limit settings, it will return an array with a single element with the original string unchanged.

```php
$items = explode(',', '2,4,6,8');

foreach ($items as $item) {
	echo $item . '<hr>';
}
```

Other resources
------------

[Official explode documentation](https://www.php.net/manual/en/function.explode.php)
