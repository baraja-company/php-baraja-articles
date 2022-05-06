PHP function array_merge()
==========================

> id: b6f27bc4-d565-4a43-91b4-c5083596cc03
> slug:
> 	cs: funkce-array-merge
> 	en: php-function-array-merge
> 
> publicationDate: '2020-02-06 09:44:11'
> mainCategoryId: '59874540-664b-4474-9869-7e6742ab6051'
> sourceContentHash: '6bdc2452d20f74f7891ba7f913a08d8f'

Available in all versions of PHP

Joins two or more fields together to create one large field.

Parameters
---------

| Parameter | Data Type | Default Value | Note |
|-----------|------------|-----------------|----------|
| `$array1` | `array` | *not* | Base array for merge. |
| `$array2` | `array` | `null` | Second merge field. |
| `$_` | `array` | `null` | nth merge field. |

We can pass any number of arrays, but at least two.

Return values
----------------

This function returns a merged array of type `array`.

Additional resources
------------

<a href="/merging-large-arrays">Merging large arrays in PHP</a>, useful for merging many arrays in a loop.

[Official manual](https://www.php.net/manual/en/function.array-merge.php)
