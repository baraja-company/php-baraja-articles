PHP funkce flock()
==================

> id: "19bac5b5-20b3-44be-8e79-6f626449fa4c"
> slug:
> 	cs: funkce-flock
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Portable advisory file locking


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$handle` | `resource` | *není* | An open file pointer. |
| `$operation` | `int` | *není* | operation is one of the following: LOCK_SH to acquire a shared lock (reader). @param int $wouldblock [optional] <p> The optional third argument is set to true if the lock would block (EWOULDBLOCK errno condition). |
| `$wouldblock` | `int` | null | The optional third argument is set to true if the lock would block (EWOULDBLOCK errno condition). |


Návratové hodnoty
----------------

`bool`

vrátí `true` v případě úspěchu, jinak `false` v případě neúspěchu.

Další zdroje
------------

[Oficiální dokumentace funkce flock](https://www.php.net/manual/en/function.flock.php)
