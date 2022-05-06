Data types in PHP
=================

> id: cd63818c-259a-484e-b006-86c35b362019
> slug:
> 	cs: datove-typy
> 	en: data-types-in-php
> 
> perex: 'Datové typy v PHP: String, integer, float, boolean, pole (array), objekt a další.'
> publicationDate: '2019-08-23 15:44:28'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: fcb2c647973c5f94e601c789b0a882f1

All data processed in PHP is of a certain type. For example, an integer, a string or a boolean (true/false).

Basic data types
--------------------

Basic types are also called **primitive data types**, or <a href="/function-is-scalar">scalar types</a>.

| Type | Name | Description |
|---------|-----------------|-------|
| `int` | Integer | (`integer`) Contains only integer. The maximum value is determined by the number of bits. On 32-bit PHP the range is from `-2,147,483,648` to `-2,147,483,647` (~ ± 2 billion), 64-bit PHP has a range from `-9,223,372,036,854,775,808` to `-9,223,372,036,854,775,807` (~ ± 9 quintillion). The maximum value can always be obtained by calling the constant `PHP_INT_MAX`. If the maximum value of an integer is exceeded, PHP will represent the number as `float` and automatically overwrite it.
| `float` | Floating Point Decimal | This is a variant of floating point number for which the rule *"the smaller the more accurate "* applies. The number is stored internally as a so-called **mantissa** and **exponent**, so you are actually storing 2 numbers between which the operation `mantissa * (2^exponent)` is performed, making it possible to store a really huge range of numbers. This uses the principle that for large numbers we don't always need to know their exact value, but we want to save as much memory as possible. Numbers of the type `float` do not need to be stored exactly and should not be used to calculate money.
| `string` | String | Contains a sequence of characters that are delimited by quotes or apostrophes. The maximum length is limited only by the capacity of the operating memory. A string can be stored in any encoding, contain emoji or binary data.
| `bool` | Boolean | (`boolean`) Boolean value from boolean algebra, can only contain `true` or `false`.
| `null` | Empty value | The empty value `null` is useful for cases where we want to express that something does not exist. For example, an article has no category. Sometimes `null` is incorrectly substituted for null (`0`) or empty strings (`''`), but this is not a good solution for expressing non-existence.

**Warning:** The `null` type is not scalar.

Zero (0) vs. null
----------------

Many developers have a hard time understanding the difference between `0` (zero) and `null` (a non-existent value) when starting development.

This distinction can be explained very well and humorously with the following image:

<img src="{$baseUrl}/images/0-vs-null.jpg" alt="0 vs. null" class="w-100 mb-3">

Manual repacking
--------------------

Some types can be converted between each other. The target data type is specified using round brackets and can be specified anywhere in the application, for example when listing a value.

For example:

```php
$pi = 3.14;

echo $pi; // prints 3.14

echo (int) $pi; // prints 3
```

Dynamic overflow
---------------------

Consider the following 2 variables:

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

> **Note:** It is important to note that we cannot convert data types completely arbitrarily, so this is not always possible. If you overwrite a data type to some other (incompatible) one, either the conversion may not occur at all, or the original content may be corrupted or completely destroyed and replaced with another one. For example, if you rewrite a string to an integer (and some text that is not a number is stored in the variable), the value `1` will be stored in the variable instead of the numeric value.

Comparing types
----------------

When comparing values, you need to consider different data types.

In general, the `==` operator is used for general comparison of two values (regardless of types), while `===` compares both values and data type.

For example:

```php
$a = '';
$b = null;

if ($a == $b) {
    // It will be evaluated as TRUE because
    // the data type conversion will occur.
}

if ($a === $b) {
    // Performs a much more rigorous validation
    // and fails because it's a different
    // content and a different data type, so
    // this code will never run.
}
```
