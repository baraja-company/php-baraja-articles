PHP function array_search()
===========================

> id: d4edadb2-ba8b-4c7f-957e-98e148f4ffbf
> slug:
> 	cs: funkce-array-search
> 	en: php-function-array-search
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '59874540-664b-4474-9869-7e6742ab6051'
> sourceContentHash: '0623711dda22d536728d03a39db503dc'

Availability in versions: `PHP 4.0.5`

Searches the array for a given value and returns the corresponding key if successful


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$needle` | `mixed` | *not* | The searched value. |
| `$haystack` | `array` | *not* | The array. |
| | `$strict` | `bool` | null | If the third parameter strict is set to true then the array_search function will also check the types of the needle in the haystack. |


Return values
----------------

`mixed`

the key for needle if it is found in the
array, false otherwise.
</p>
<p>
If needle is found in haystack
more than once, the first matching key is returned. To return the keys for
all matching values, use array_keys with the optional
search_value parameter instead.

Additional resources
------------

[Official array-search documentation](https://www.php.net/manual/en/function.array-search.php)
