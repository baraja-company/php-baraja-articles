The eval() function in PHP
==========================

> id: '861deebf-ad95-4b12-90df-62664843147e'
> slug:
> 	cs: funkce-eval
> 	en: the-eval-function-in-php
> 
> publicationDate: '2020-02-16 18:37:20'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '9e8960ba9bb3b92cd606e796660e7e30'

The `eval` function is used to execute the passed string as PHP code.

PHP language design and practical features
---------------------------------------

PHP is an interpreted language, which means in particular that its code is evaluated by an **interpreter**, a special type of program that reads the code you write and evaluates it directly from the string in real time. Other languages (such as C) must be compiled into machine code before they can be run.

Because PHP is interpreted, there is a way to change exactly what will be evaluated at runtime and even compile the code dynamically, which is exactly what `eval()` is good for.

Use at your own risk!
---------------------------------

Only use the `eval` function when you know exactly what you are doing! This means, in particular, that you have checked all user input and no security breaches can occur. This is because if a user manages to sneak his string into the `eval` function, it will be evaluated as real code and can, for example, delete the entire site, steal the database or gain control of the entire server.

A real-world example
--------------

There are not many good examples where `eval` can be used, because **practically there is always a better way** to solve the problem.

For example, it can be used when evaluating expressions:

```php
// User query
$query = '5 + 3 * 2';

// Process the expression as regular PHP code
eval('$result = @(' . $query . ');');

// Extract a variable with the solution to the expression
echo $result; // prints 11
```

For details, see <a href="/pokrocila-kalkulacka">Calculator in PHP: Processing a mathematical expression as a string</a>.

Use to render templates
-----------------------------

Sometimes `eval` is used to evaluate generated code, typically compiled templates.

However, as mentioned, each case can be handled differently and better, and in this case it makes more sense to save the serialized template in a separate PHP file and load it via `require` or `include`. In addition to having full control over the content of the template, it will also remain physically on disk, which supports improved application performance due to the caching capability.
