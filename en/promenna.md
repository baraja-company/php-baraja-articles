Variables in PHP
================

> id: b89774e7-143c-4a8c-8dc6-3b3d2c78d5b7
> slug:
> 	cs: promenna
> 	en: variables-in-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '23e4d537f18a3b2e1946c79be1aacf52'

This page is a complete summary of how variables work in PHP. The text is written in a slightly technical style and may not be fully understood by beginners. If you're interested in the complete basics, then read <a href="/first-script">the beginner's tutorial</a> and <a href="/principles-of-prominent-script">principles of writing variables</a>.

Description
-----

A variable is a virtual location in operational memory, it is defined by `name` and `data type`. Within the data type, the variable then has some `content`.

Internally, PHP represents variables as a so-called hash table, i.e. all variables are stored in a table that is very fast to search, so the time required to access each variable is *almost* constant.

Writing examples:

```php
$a = 10;
$b = 'cat';
$c = true;
```

Each line in the sample denotes the definition of one variable. The name of each variable starts with the dollar sign `$` followed by the name itself. The equals sign can be used to assign a value to the variable.

Internally, the variables will be stored in memory as a hash table:

| Name | Type | Abbreviation | Value |
|-------|---------|---------|---------|
| `$a` | integer | int | `10` |
| `$b` | string | string | `cats` |
| `$c` | boolean | bool | `true` |

Variable types
---------------

Variables are classified according to access rights and usage:

- <a href="/local-variable">local variables</a> (valid within context, i.e., within functions and methods),
- <a href="/global-variable">global variables</a> (valid within the entire script),
- <a href="/superglobal-variable">Superglobal variables</a> (valid within the entire server environment - typically user data),
- <a href="/promenna-variable">variable variables</a> (a special variable that contains a reference (link) to another variable).

* `Global variables` and `variable variables` should not be used because they contribute to code unreadability and "magical" (unexpected) application behavior.*

Allowed contents of variables
--------------------------

Variables can contain anything that their current data type allows. If the data type is not specified, it will be determined automatically, based on the content (this is not recommended, as it can lead to errors).

Datatypes work independently, so we can use almost any type. However, if we want to perform some merge operation, we must always ensure conversion to one format.

A good example is for example addition and multiplication of numbers:

```php
$x = 5; // integer
$y = 3; // integer
$z = $y + $y; // the variable $z will be composed based on multiple variables
```


In this case, PHP is faced with the question of what data type the newly created variable `$z` will have. If they are the same data type and the operation is possible, the data type is inherited.

However, sometimes we can perform an operation with multiple data types:

```php
$x = 1; // integer
$y = 3.14159; // float
$z = $y + $y; // float
```


In this case, we merge integer and float. The output will be a decimal number, so float is used. In this case, PHP will do something called **dynamic repartitioning**.

However, we can't always rely on this behavior. For example, how would you like to merge a number and a string?

```php
$x = 256; // integer
$y = 'Hi!'; // float
$z = $y + $y; // ????
```

Data types (overview of the most important ones)
--------------------------------------

PHP is an interpreted language, so it has some peculiarities compared to other programming languages, one of them is that we don't need to specify data types for variables, i.e. each variable dynamically changes its data type according to its content (unless otherwise stated). Still, it is good to know at least the basic data types, especially useful when optimizing applications or working with databases.

The notation can look like this:

```php
$x = (int) 25; // creates a variable of type integer
```

<a href="/datove-types">Data type overview</a>.

Data type inheritance
-----------------------

What data type will the variable `$x` have if we only know this piece of source code?

```php
$x = $y;
```

It depends on the data type of the variable `$y` from which both the value and its data type will be inherited. In this case, we do not know the variable `$x`, so we cannot continue evaluating the code and an error message would be thrown.

Dynamic override
---------------------

Let's have the following 2 variables:

```php
$x = 10;
$y = '10';
```

What is the difference between the contents of the variables `$x` and `$y`?

The variable `$x` is a number, `$y` is a string (containing a "1" and a "0"), so if we just store the variable in memory and do not perform any operation that will affect the value. For example, the following 2 entries will return the same result:

```php
echo $x + 5; // prints 15
echo $y + 5; // prints 15
```

In the second case, the so-called **dynamic rewriting** occurs, i.e. the variable converts its data type so that a computational operation can be performed on it. This behaviour cannot always be relied upon and is more of a corrective behaviour to fix poorly written beginner scripts. If possible, always write numbers with a data type to store the numbers, as this increases their precision and makes them easier to use in the future.

> **Note:** It is important to note that we cannot convert data types completely arbitrarily, so this is not always possible. If you overwrite a data type to some other (incompatible) one, either the conversion may not happen at all, or the original content may be corrupted or completely destroyed and replaced with another one. For example, if you rewrite a string to an integer (and some text that is not a number is stored in the variable), the value `1` will be stored in the variable instead of the numeric value.

Representing strings as arrays
------------------------------

All strings are stored internally as an array of characters. That is, each character has its own **index** and can be referenced. If we don't specify an index, the whole string is worked with.

```php
$x = 'Let's program in PHP!';
$n = 3;

echo $x; // this prints the entire contents of the $x variable
echo $x[0]; // this prints the null character of the variable $x
echo $x[$n]; // this will print the nth character of the variable $x
```

> **Note:** PHP numbers from zero, i.e. the zero character is 'P' and the first character is 'r'.
>
> Additionally, the characters are byte-switched. For example, the character "no" in UTF-8 encoding is 2 bytes long, so the character index in the string will not match the real position when scrolling and 2 indices will be used to store the character.

The existence of an array element should always be verified with the `isset()` function:

```php
if (isset($x[$n])) {
    echo $x[$n];
}
```

Alternatively, this can be written nicely with the ternary operator:

```php
echo $x[$n] ?? '';
```

Copying variables
---------------------

Consider the following variable:

```php
$q = 'Lorem ipsum, ...';
```

And then copy its value to the next variable:

```php
$qi = $q;
```

Fortunately, no copying will be done and PHP will just store a reference to the value in a **hash table**. The value will only be actually copied when the value of one of the variables should change. This behavior is handled by a component generally called **Garbridge collector**.
