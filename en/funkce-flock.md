PHP function flock()
====================

> id: '19bac5b5-20b3-44be-8e79-6f626449fa4c'
> slug:
> 	cs: funkce-flock
> 	en: php-function-flock
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '58980d964417a1160af7b174ef84cb4a'

Availability in `PHP 4.0`

Portable advisory file locking


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$handle` | `resource` | *not* | An open file pointer. |
| `$operation` | `int` | *not* | operation is one of the following: LOCK_SH to acquire a shared lock (reader). @param int $wouldblock [optional] <p> The optional third argument is set to true if the lock would block (EWOULDBLOCK errno condition). |
| `$wouldblock` | `int` | null | The optional third argument is set to true if the lock would block (EWOULDBLOCK errno condition). |


Return values
----------------

`bool`

Returns `true` on success, otherwise `false` on failure.

Other resources
------------

[Official documentation for the flock function](https://www.php.net/manual/en/function.flock.php)
