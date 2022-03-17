PHP funkce is_callable()
========================

> id: f5195404-44c1-455d-85a0-31f75c3a5103
> slug:
> 	cs: funkce-is-callable
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0.6`

Verify that the contents of a variable can be called as a function


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$name` | `callback|callable` | *není* | Can be either the name of a function stored in a string variable, or an object and the name of a method within the object, like this: array($SomeObject, 'MethodName') |
| `$syntax_only` | `bool` | null, | If set to true the function only verifies that name might be a function or method. It will only reject simple variables that are not strings, or an array that does not have a valid structure to be used as a callback. The valid ones are supposed to have only 2 entries, the first of which is an object or a string, and the second a string. |
| `$callable_name` | `string` | null | Receives the "callable name". In the example below it is "someClass::someMethod". Note, however, that despite the implication that someClass::SomeMethod() is a callable static method, this is not the case. |


Návratové hodnoty
----------------

`bool`

true if name is callable, false
otherwise.

Další zdroje
------------

[Oficiální dokumentace funkce is-callable](https://www.php.net/manual/en/function.is-callable.php)
