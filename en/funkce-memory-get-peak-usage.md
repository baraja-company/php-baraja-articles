PHP function memory_get_peak_usage()
====================================

> id: a3a5f2b2-21e3-4d2e-8c1d-3a8af523b652
> slug:
> 	cs: funkce-memory-get-peak-usage
> 	en: php-function-memory-get-peak-usage
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: ab12b253-41a0-4bf3-9019-956608d7d534
> sourceContentHash: '4955165aa9a0fb949759ad6e536931d2'

Availability in versions: `PHP 5.2.0`

Returns the peak of memory allocated by PHP


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$real_usage` | `bool` | null | Set this to true to get the real size of memory allocated from system. If not set or false only the memory used by emalloc() is reported. |


Return values
----------------

`int`

the memory peak in bytes.

Other resources
------------

[Official documentation for memory-get-peak-usage](https://www.php.net/manual/en/function.memory-get-peak-usage.php)
