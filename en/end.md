End()
=====

> id: '879c7c9a-a497-4080-865d-f15e6c9e8578'
> slug:
> 	cs: end
> 	en: end
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '2d097dd1fae4e4f125d879c92bc2cfbe'

- Support PHP 4, PHP 5
- Short description: sets the internal pointer of an array to its last element.
- Requirements.

Description
--------------------------

The `end()` function sets an internal array pointer to the last element and returns its value.

Similar functions
--------------------------

- `current()`
- `each()`
- `prev()`
- <a href="/reset">reset()</a>
- `next()`

Example
--------------------------

```php
echo end([
    'apple',
    'banana',
    'cranberry',
]); // prints cranberry
```

```php
$a = [];
$a[1] = 1;
$a[0] = 0;

echo end($a); // prints 0
```

Parameters
--------------------------

| # | Type | Description |
| --- | ------- | ----- |
| 1 | `array` | The name of the array to work with. |

Return values
--------------------------

Returns the value of the last element or `false` for an empty array.

Differences from earlier versions
--------------------------

None
