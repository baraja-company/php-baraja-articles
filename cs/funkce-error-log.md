PHP funkce error_log()
======================

> id: f8454528-e7d2-4784-8a40-ba6249988167
> slugCS: funkce-error-log
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Send an error message somewhere


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$message` | `string` |  | The error message that should be logged. |
| `$message_type` | `int` | null, | Says where the error should go. The possible message types are as follows: |
| `$destination` | `string` | null, | The destination. Its meaning depends on the message_type parameter as described above. |
| `$extra_headers` | `string` | null | The extra headers. It's used when the message_type parameter is set to 1. This message type uses the same internal function as mail does. |


Návratové hodnoty
----------------

`bool`

true on success or false on failure.

Další zdroje
------------

https://php.net/manual/en/function.error-log.php
