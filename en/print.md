Print
=====

> id: '23d672c5-b347-43b8-bd08-8ccb7ecca33f'
> slug:
> 	cs: print
> 	en: print
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7f7c3c32d9f3c1efd794ca38b7db384

`print` - String output

Description
--------------------------

```php
print 'Hello World!';
```

`print()`, is not really a real function (it's a language construct), so you don't need to use parentheses.

Parameters
--------------------------

- **arg**

Output parameter

Return values
--------------------------

Always returns the number 1.

Note
--------------------------

Note: Because this is a **language construct** (not a function), it cannot be loaded into a variable.

Example
--------------------------

```php
print "hello world";

print "print can output multiple lines of text.But watch out for the HTML tag
because it will not print. That's what the <a href="/nl2br">Nl2br</a> function is for.";

// Example of a connection to a variable
$a = 'php';

print 'I like ' . $a; // I like php
```

**print** is exactly the same function as **echo**. If you're looking for more information, check out this article on the <a href="/echo">echo</a> function.
