PHP function var_dump()
=======================

> id: '3afb12dc-e91f-4126-9f64-e350449e095c'
> slug:
> 	cs: funkce-var-dump
> 	en: php-function-var-dump
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '1f0c27cc2d2c69fa0d3dc417ac5fe5e9'

Availability in `PHP 4.0`

Prints variable information directly to the output (page).

Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$expression` | `mixed` | *not* | Variable to be output |
| `$_` | `mixed` | null | |


Return values
----------------

`void`

The function returns nothing. It renders the output directly to the page like `echo`.

```php
var_dump('I like PHP'); // string(13) "I like PHP"
```

Example of a more complex structure:

```php
var_dump([1, 2, 3]);
```

Returns the following:

```
array(3) {
  [0]=>
  int(1)
  [1]=>
  int(2)
  [2]=>
  int(3)
}
```

Other resources
------------

[Official var-dump documentation](https://www.php.net/manual/en/function.var-dump.php)
