Pure functions in PHP
=====================

> id: d94c18a9-8bc4-4377-8042-e7c8b48320a2
> slug:
> 	cs: pure-funkce
> 	en: pure-functions-in-php
> 
> publicationDate: '2021-10-27 10:30:00'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: e47a3507ddb189a218ad8e3a38703e5c

In functional programming, there is a concept of a **pure function**, which refers to a function that always returns the same output to the same input (i.e. is deterministic), and at the same time does not suffer from any side-effects (i.e. does not affect its environment).

What a pure function looks like
----------------------

Example of a pure function:

```php
// This is a pure function
function add(int $a, int $b): int
{
	return $a + $b;
}
```

This is a pure function because the output is always the same based on the input arguments.

What is not a pure function
-------------------

```php
// This is an impure function
function add(int $a, int $b): int
{
	echo 'Adding...';
	file_put_contents('file.txt', 'value: ' . $a);
	return $a + $b;
}
```

This type of function is not pure because the function changes the file system. Another type of impure function is when it interacts with the database, prints to the screen, and so on.
