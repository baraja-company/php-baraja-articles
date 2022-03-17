PHP funkce substr_replace()
===========================

> id: "63d688c9-bdd6-43ed-a1e8-44815c4a9f8e"
> slug:
> 	cs: funkce-substr-replace
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Replace text within a portion of a string


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$string` | `mixed` | *není* | Zpracovávaný řetězec. |
| `$replacement` | `string` | *není* | The replacement string. |
| `$start` | `int` | *není* | If start is positive, the replacing will begin at the start'th offset into string. |
| `$length` | `int` | null | If given and is positive, it represents the length of the portion of string which is to be replaced. If it is negative, it represents the number of characters from the end of string at which to stop replacing. If it is not given, then it will default to strlen( string ); i.e. end the replacing at the end of string. Of course, if length is zero then this function will have the effect of inserting replacement into string at the given start offset. |


Návratové hodnoty
----------------

`mixed`

The result string is returned. If string is an
array then array is returned.

Další zdroje
------------

[Oficiální dokumentace funkce substr-replace](https://www.php.net/manual/en/function.substr-replace.php)
