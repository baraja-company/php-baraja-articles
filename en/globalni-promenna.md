Global variables in PHP
=======================

> id: '1cdfc51a-31f4-4a5d-81f6-cfd92f86a9d4'
> slug:
> 	cs: globalni-promenna
> 	en: global-variables-in-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '31e78a9abf19252ef62315230a3731b5'

Global variables are available at any time in any part of the application and do not need to be passed.

> **Warning:** A well-designed application should not use global variables because they violate the encapsulation principle and can cause hard-to-detect errors if handled carelessly.

Example usage:

```php
$a = 1;
$b = 2;

function sum(): void
{
	global $a, $b;
	$b = $a + $b;
}

sum();

echo $b; // prints the number 3, because the variable $b is global
```

Note that we have retrieved the variable `$a` and `$b` outside their natural context. This behavior is referred to as "magical" because if another function overrides the variables currently in use, the application will experience an unexpected condition.

Properly, the application should **encapsulate** and pass the variables each time:

```php
$a = 1;
$b = 2;

function sum(int $a, int $b): int
{
	return $a + $b;
}

echo sum($a, $b); // prints 3
```

This allows us to call the function dynamically with different input parameters and its output will depend only on the inputs, not the environment.

Getting input parameters from URL
---------------------------------

Perhaps the only sensible use of global variables is in parsing user input, in which case we're talking about <a href="/superglobal-variable">superglobal variables</a>.

In this case, it is a clean design because the variable should be read-only, not write-only, and moreover, it is the same throughout the application:

```php
function getNameFromUrl(): string
{
    return isset($_GET['name'])
    	? htmlspecialchars($_GET['name'])
    	: '';
}

echo getNameFromUrl();
```
