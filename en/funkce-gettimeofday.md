PHP function gettimeofday()
===========================

> id: '30d09f0c-d4ea-4717-9d52-a674c79f0342'
> slug:
> 	cs: funkce-gettimeofday
> 	en: php-function-gettimeofday
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '63bfbf4c154dcd7f32e9b5ebf00b5718'

Availability in `PHP 4.0`

Get current time


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$return_float` | `bool` | null | When set to true, a float instead of an array is returned. |


Return values
----------------

`mixed`

By default an array is returned. If return_float
is set, then a float is returned.
</p>
<p>
Array keys:
"sec" - seconds since the Unix Epoch
"usec" - microseconds
"minuteswest" - minutes west of Greenwich
"dsttime" - type of dst correction

Other resources
------------

[Official gettimeofday documentation](https://www.php.net/manual/en/function.gettimeofday.php)
