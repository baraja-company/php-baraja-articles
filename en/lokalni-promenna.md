Local variables in PHP
======================

> id: '9c3fbf4e-8612-4599-8127-d66b9ec5e980'
> slug:
> 	cs: lokalni-promenna
> 	en: local-variables-in-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: cffee22c4dbcc2ec331b73235349c409

Local variables are valid only inside the body of **<a href="/prikazy-a-function">function</a>** or **method** (in object-oriented programming).

If we are working in the context of a regular script, everything happens as expected:

```php
$x = 5;

echo $x; // prints 5
```

But when we define our own function, the behavior changes slightly:

```php
$x = 5;

function myFunction(): int
{
    $x = 3;

    echo $x; // prints 3
}

echo $x; // prints 5
```


The reason is that the $x variable exists only in the context of the current function or method. This behavior is intentional.

If we want to pass a value from the surrounding code to a function, we have to call it with the necessary parameters:

```php
echo myFunction(5); // prints 6

function mojeFunction(int $x): int
{
    return $x + 1;
}
```


Values are passed into functions using parameters. If you want to pass additional variables into the function beyond the parameters, you can use <a href="/global-variable">global variables</a>, but this is not a good idea.

> Using local variables makes a huge difference when programming a larger application. If we didn't distinguish the validity of variables across contexts, it would be impossible to guarantee that a variable wouldn't be overridden in a place where we don't count on it (which is why global variables are evil).

```php
$x = 5;
$y = 3;

function sum(int $x, int $y): int
{
    return $x + $y;
}

echo $x; // prints 5
echo sum($x, $y); // prints 8
```
