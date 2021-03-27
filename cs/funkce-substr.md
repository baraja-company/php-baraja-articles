PHP funkce substr()
================================

> id: f74977bc-9004-4210-84a1-b8d0e8352795
> slugCS: funkce-substr
> publicationDate: 2019-09-11 10:04:04
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 4.0`

Return part of a string


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$string` | `string` |  | The input string. |
| `$start` | `int` |  | If start is non-negative, the returned string will start at the start'th position in string, counting from zero. For instance, in the string 'abcdef', the character at position 0 is 'a', the character at position 2 is 'c', and so forth. |
| `$length` | `int` | null | If length is given and is positive, the string returned will contain at most length characters beginning from start (depending on the length of string). |


Návratové hodnoty
----------------

`string`

|bool the extracted part of string or false on failure.

Další zdroje
------------

https://php.net/manual/en/function.substr.php
