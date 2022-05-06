PHP function time_nanosleep()
=============================

> id: '481733e0-3060-4750-b1e8-c3ef63988d0a'
> slug:
> 	cs: funkce-time-nanosleep
> 	en: php-function-time-nanosleep
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '7e194bfe898362d00aa75709dba27a68'

Availability in `PHP 5.0`

Delay in seconds and nanoseconds


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$seconds` | `int` | *not* | Must be a positive integer. |
| `$nanoseconds` | `int` | *not* | Must be a positive integer less than 1 billion. |


Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.

If the delay was interrupted by a signal, an associative array with components will be returned:

- seconds - the number of seconds remaining in the delay.
- nanoseconds - the number of nanoseconds remaining in the delay

Other resources
------------

[Official documentation of the time-nanosleep function](https://www.php.net/manual/en/function.time-nanosleep.php)
