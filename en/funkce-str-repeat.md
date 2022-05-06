PHP function str_repeat()
=========================

> id: b9749c0e-3003-4b92-846e-40654974d273
> slug:
> 	cs: funkce-str-repeat
> 	en: php-function-str-repeat
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '921f8716a902fbb7f3a3c7884b2e5359'

Availability in `PHP 4.0`

Repeats string.

Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$input` | `string` | *not* | String to be repeated |
| `$multiplier` | `int` | *not* | How many times to repeat |


Return values
----------------

`string`

The final repeated string.

```php
echo 'A' . str_repeat('a', 5) . 'hi'; // Aaaaaahoy
```

Other resources
------------

[Official str-repeat documentation]([Official str-repeat documentation](https://www.php.net/manual/en/function.str-repeat.php))
