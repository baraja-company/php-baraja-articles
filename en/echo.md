Echo - output to source code
============================

> id: d6a1a7bf-03bf-49ea-9947-d4d6753ca6e4
> slug:
> 	cs: echo
> 	en: echo---output-to-source-code
> 
> perex: Echo - výpis řetězce do stránky a na standardní výstup. Možnosti escapování a HTTP hlaviček.
> publicationDate: '2020-02-16 18:40:31'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '7d611691825ec537f58f387696d13e28'

The `echo` construct is used to dump a variable or string into the source code.

| Support: | All versions
|----------------|------
| Brief description: | Output one or more strings
| Type: | <a href="/commands-and-functions">command, construct</a> (not a function)

Description
-----

```php
echo 'hello world';
```

Print "hello world".

```php
$var = 'text';
echo $var;
```

Print the value of the variable `$var`, i.e. "Text".

**Echo** is not a function (it's a <a href="/commands-and-functions">command</a>), so you may or may not use the parenthesis. Thus, writing `echo ('hello world');` is also correct.

> **Extra note:** PHP treats **Echo** as a command (a construct) and thus treats it as an expression. The parenthesis is optional in this case. If we give the notation: `echo ('something');`, the **Echo** statement does not become a function and is not treated as such. The parenthesis in this case means enclosing the exact value of the expression, similar to how it works in mathematics.

Quotation marks
--------

Strings can be enclosed in quotation marks and apostrophes.

So this:

```php
echo "hello";
```


It's the same as this:

```php
echo 'hello';
```


But beware that each string must start and end with the same type of quote character and this quote character must not be used in the string.

For example, if you want to output an HTML link (or any HTML code), you must precede the quotation mark with a slash. A slash means "exactly this character", so it is not understood as an expression in the language.

```php
echo "<a href="index.php">link text</a>";
```


Technical note: Quotation marks have a <a href="/quotation-meaning">special meaning</a> in PHP.

Parameters
---------

- `arg` Output parameter.

Return values
-----------------

No value is returned.

Cannot be used as a variable.

Note
--------

Note: Because this is a **language construct** (construct = command) (not a function), it cannot be loaded into a variable.

Example
-------

```php
echo "hello world";

echo "echo can output multiple lines of text.
But beware of the HTML tag <br>, it will not be printed. That's what the nl2br() function is for.";

$a = "php"; // variable definition

echo "I like " . $a; // Prints: I like php
```

**Echo** also has a shortened syntax, where you can use only the equals sign after the opening php tag.

````php
Hi <?=$name;?>!
```

This is useful if we need to dump some quick information into the page. For example, the current year:

````php
By Jan Barášek © <?=date('Y');?>
```

> This shortened syntax will only work if shortened opening php tags are enabled, i.e. the `short_open_tag` directive is set to `on`.

Operation
-------

All common mathematical operations can be performed within the **echo** command.

For a detailed discussion of mathematics, see <a href="/mathematics">a separate article</a>.

```php
echo 5 + 3 * 2; // prints 11
```
