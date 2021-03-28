PHP funkce strtok()
================================

> id: c76927b3-7df7-4fc2-86d2-a11d72cdd863
> slugCS: funkce-strtok
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Tokenize string
Note that only the first call to strtok uses the string argument.
Every subsequent call to strtok only needs the token to use, as it keeps track of where it is in the current string.
To start over, or to tokenize a new string you simply call strtok with the string argument again to initialize it.
Note that you may put multiple tokens in the token parameter.
The string will be tokenized when any one of the characters in the argument are found.


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$str` | `string` | null, | The string being split up into smaller strings (tokens). |
| `$token` | `string` |  | The delimiter used when splitting up str. |


Návratové hodnoty
----------------

`string`

A string token.

Další zdroje
------------

https://php.net/manual/en/function.strtok.php
