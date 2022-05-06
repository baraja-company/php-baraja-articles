PHP function sleep()
====================

> id: f125cbbd-ce50-4ff2-913d-e25e03936d34
> slug:
> 	cs: funkce-sleep
> 	en: php-function-sleep
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '55cd4508d0e744a9ee61e3aa30f5f469'

Availability in `PHP 4.0`

Script sleep.

Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$seconds` | `int` | *not* | How long to sleep in seconds. |


Return values
----------------

`int`

Returns zero (`0`) if correct, or `false` if incorrect.

If the sleep was artificially interrupted, it returns the number of seconds missing until the end of the sleep.

```php
echo 'Hello!'; // Printed immediately

sleep(3); // Script will be paused for 3 seconds

echo 'How are you?'; // Printed after 3 seconds
```

Other resources
------------

[Official documentation of the sleep function](https://www.php.net/manual/en/function.sleep.php)
