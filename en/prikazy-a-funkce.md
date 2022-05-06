Commands, keywords and functions in PHP
=======================================

> id: '96a00928-4410-479d-ada0-612de21cdacd'
> slug:
> 	cs: prikazy-a-funkce
> 	en: commands-keywords-and-functions-in-php
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '4534cc03d2ae66454b9f398139f232a4'

Every PHP script consists of commands and functions, together these are called **constructs**. When the script is processed, these language expressions are tracked (this process is called **tokenization**) and based on the *keywords* the interpreter decides how the processor should behave.

Commands
--------------------------

Commands (<a href="https://www.php.net/manual/en/reserved.keywords.php">they are called **keywords** in English</a>) are already pre-programmed parts of the language, they are reserved specifically for their purpose, can be used immediately in any situation (they don't require additional special libraries or installation), and comprise the basis of all scripts.

For example, <a href="/echo">extracting a string into HTML code</a>:

```php
echo 'Echo is a language command to extract the contents of';
```

With commands, it is important to note that they do not return any value and thus cannot be used as a variable. Commands can also be identified by the fact that they do not contain parentheses.

Examples:

```php
echo 'Hello world!';
echo ('Hello world!');
```

If I enclose the contents of the command in parentheses, the behavior will not change and it will be viewed as an **expression**. It's the same as writing in math:

```php
5 + 3

// or:

(5 + 3)
```

Technically there is a difference, but in practice nothing changes.

Functions
--------------------------

If there were only commands, it would be pretty boring. Functions are a collection of multiple commands.

We often want to execute the same code in multiple places repeatedly. In that case, constant copying would be too much work and could lead to errors, so we wrap such code in a function that we name and then just call it by name.

When we call the function, we pass it the parameters as variables, it executes the code and returns the result, which we can use further.

```php
$text = 'PHP is my favorite language!';

echo 'Text '' . $text . '" is long ' . strlen($text) . ' characters.';
```

PHP has many functions directly predefined (<a href="/documentation">see the documentation for a complete list</a>), but it is often convenient to define your own:

```php
function myFunction(int $x, int $y): int
{
   $z = $x + $y; // adds the input parameters and stores them in a variable

   return $z; // returns the variable $z
}

echo myFunction(5, 3); // prints the number 8, because the function processed the numbers
```

Variables in functions
--------------------------

Local variables are used within functions, which means that they can only be used inside the function and cannot be manipulated (or defined) outside the function. They get their initial values from the function parameters directly in the function definition.

Example:

```php
$z = 5;

function prvniFunction(int $x, int $y): int
{
   return $x - $y; // this would return the difference of the numbers
}

function secondFunction(): mixed
{
   return $z; // this will return an error because the variable $z
              // is not defined inside the function
}
```

Sometimes it is useful to set one of the parameters as optional, this is done by defining an alternate (default) value:

```php
echo prictiCislo(5); // returns 6
echo prictiCislo(5, 7); // returns 12

function prictiCislo(int $x, int $y = 1): int
{
   return $x + $y;
}
```

The `prictiCislo()` function adds the value from the `$y` variable to the `$x` variable. If the variable `$y` is not defined (specified as a parameter when the function is called), its default value written in the function definition will be used. The second function parameter (the `$y` variable) is therefore optional.

> Each function can have only one output (one return), if you specify multiple outputs in a function, the one specified first will be returned. If you want to return multiple values, you must use an array.
>
> In PHP (unlike other languages) we don't have to specify a return at all, in which case the function returns nothing (void) on any input. Such functions are mostly used to store data or output to source code. In general, however, it is recommended to always return some value, at least an acknowledgement that everything went well.
