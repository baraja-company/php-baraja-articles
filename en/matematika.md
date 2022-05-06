Mathematics in PHP
==================

> id: '69a432ee-7635-46e2-bb21-8492cb62c4e6'
> slug:
> 	cs: matematika
> 	en: mathematics-in-php
> 
> perex: 'Matematika a matematické funkce v PHP. Definice čísel, operátorů a vhodné typy pro výpočty a ukládání čísel.'
> publicationDate: '2020-02-16 18:24:10'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '1070f11c80eff1faf3388e47721b01a2'

As in other languages, numbers in PHP are represented in the binary system (the system of ones and zeros), so some mathematical operations may produce slightly different results than we would expect. This page gives an overview of the behaviour of calculations in different contexts. At least a basic knowledge of **numerical techniques** and **number systems** is needed to understand it.

Representation of numbers in memory
---------------------------

The way numbers are represented is characterized by the data type, for example integer is converted directly from source code from decimal to binary. We don't have to worry about converting numbers at all, everything is done automatically.

The consequence of this conversion is that the maximum value of an integer is determined by the number of bits available in memory. On 32-bit PHP the range is from `-2,147,483,648` to `-2,147,483,647` (~ ± 2 billion), 64-bit PHP has a range from `-9,223,372,036,854,775,808` to `-9,223,372,036,854,775,807` (~ ± 9 quintillion). The maximum value can always be obtained by calling the constant `PHP_INT_MAX`.

Decimal numbers are stored in the **float** data type, which has limited precision, so it is stored internally as a pair of numbers that are subject to the mathematical operation `mantissa * (2^exponent)`. The practical consequence is that we are able to store a relatively large number (on the order of billions) in a small number of bits, just not very precisely.

The consequence of using the **float** data type is that the result of calculating `1 - 0.9` is not exactly `0.1`.

Example from <a href="https://diskuse.jakpsatweb.cz/?action=vthread&forum=9&topic=97912">web forums</a>:

```php
$x = 0.3;
$y = 0.4;
echo 0.7 - $x - $y; // prints -5.5511151231258E-17
```

However, if we adjust the numbers slightly:

```php
$x = 6.5;
$y = 7.5;
echo 14.0 - $x - $y; // prints 0
```

This issue is discussed in general in <a href="https://vtm.zive.cz/clanky/proc-pocitacum-delaji-problemy-desetinna-cisla/sc-870-a-185672/default.aspx">a separate article on VTM.e15.cz: Why computers have problems with decimal numbers</a>, or in general about <a href="https://cs.wikipedia.org/wiki/Pohyblivá_řádová_čárka">floating point on Wikipedia</a>.

> Generally, it is a good idea to round the result after the calculation, or avoid decimals altogether. For example, store the price not in crowns (like 14.90) but in pennies (like 1490) and then work with it exactly. Rounding is discussed in the next section of this article.

Mathematical operations
-------------------

```php
$a = 5;
$b = 3;

echo $a + $b;
```

Common mathematical conventions can be used for operations, which respect the precedence of operators according to the rules of mathematics (multiplication takes precedence over addition, etc.). Values can be written directly or read in via variables.

> It may not be obvious at first glance, but in the math example, the equals sign (`=`) is not written. Hence, it follows that only simple `binary operations` can be solved, which always work with only `two elements` (don't count equations, you have to use a specialized library for that).

Overview of basic operators
----------------------------

> In the `result` column, I list what is printed assuming:

```php
$a = 5;
$b = 3;
$c = 10;
```

| Operation | Mark | Mark in word | Example of notation | Result
|-------------------|-----------|---------------|-----------------------|----------
| Addition | `+` | Plus | `echo $a + $b;` | 8
| Subtraction | `-` | Minus | `echo $a - $b;` | 2
| Multiplication | `*` | Asterisk | `echo $a * $b;` | 15
| division | `/` | slash | `echo $a / $b;` | 1.6666666666667
| brackets | `( )` | brackets | `echo $a + ($b * $c);`| 35
| String concatenation | `.` | Period | `echo $a . $b . $c;` | 5310
| Remainder after division | `%` | Percent | `echo $a % $b;` | 2
| Adding one | `++` | two pluses | `echo $a++;` | 6
| Subtract one | `--` | two minus | `echo $a--;` | 4
| Power | `**` | two asterisks | `echo $a ** $b;` | 125

> The power operator (double asterisk) is only available from PHP 7.1 onwards. In other versions of PHP you must use the universal function `pow($a, $b)`.

Overview of basic mathematical functions
---------------------------------------

If you are solving some common computational task, most likely there is already a function directly in PHP, here is an annotated overview of them.

Rounding
--------------

Normal rounding is done with the <a href="/function-round">round()</a> function, it has 3 parameters:

- Rounded number
- Optional: Precision (to how many decimal places)
- Optional: Halving value (from what value to round up)

```php
round(5); // 5
round(5.1); // 5
round(5.4); // 5
round(5.5); // 6
round(5.8); // 6
round(5483.47621, 2); // 5483.47
round(5483.47621, -2); // 5500
```

There is also a <a href="function-floor">floor()</a> function for rounding down and a <a href="/function-ceil">ceil()</a> function for rounding up.

> **Watch out for data types!**
>
> All rounding functions return the result as `float`, even if the result is an integer.
>
> > If you want to treat the result as an integer, it is important to overtype the result. For example: `echo (int) round(3.14);`

PHP works with different data types, for example with **float** it can easily happen that the result of `round()` is not a number with finite decimal expansion. For output, I recommend using the `number_format()` function, which I discuss below.

Goniometric functions
--------------------

These are used for a lot of different calculations and are based on the unit circle. They are used, for example, in plotting circles, ellipses, displacements and so on.

- <a href="/function-sin">sin()</a>
- <a href="/function-cos">cos()</a>
- <a href="/function-tan">tan()</a>

Cyclometric functions
--------------------

Calculate the angle from `x` and `y`. Conversion of Cartesian and polar coordinates.

- <a href="/function-asin">asin()</a>
- <a href="/function-acos">acos()</a>
- <a href="/function-atan">atan()</a>

Hyperbolic functions
-------------------

Based on the unit hyperbola. Their definitions can be easily described by using similes:

- Ellipse, Circle, Circle with radius 1
- Hyperbola, Equiaxial hyperbola, Unit hyperbola

They are used, for example, in terrain generation and physical simulation.

- <a href="/function-sinh">sinh()</a>
- <a href="/function-cosh">cosh()</a> (chainline)
- <a href="/function-tanh">tanh()</a>

Hyperbolometric functions
------------------------

Based on the unit hyperbola.

- <a href="/function-asinh">asinh()</a>
- <a href="/function-acosh">acosh()</a>
- <a href="/function-atanh">atanh()</a>

Examples of using goniometric functions
---------------------------------------

```php
echo sin(30); // -0.98803162409286
echo sin(deg2rad(30)); // 0.5
echo cos(deg2rad(123)); // -0.54463903501503
echo 1/tan(deg2rad(45)); // 1 (onen cotangent)
echo sin(deg2rad(500)); // 0.64278760968654
echo atan2(deg2rad(50), deg2rad(23)); // 1.1396575860761
```

Calculator - mathematical expression processing
--------------------------------------------

Sometimes it may happen that we need to process a mathematical expression as a user string, for example `5+2^(1+3/2)`.

There is no easy way to process such input in PHP, but I have programmed a series of libraries that address this issue.

For details, see the separate article <a href="/pokrocila-calculator">Calculator in PHP: Processing a mathematical expression as a string</a>.

Operations with large numbers and accuracy of calculations
------------------------------------------

Large numbers and decimals are stored as floats in PHP, plus they are rounded to about 15 decimal places, which can sometimes be annoying. This is why the **<a href="https://www.php.net/manual/en/book.bc.php">BCMath Arbitrary Precision Mathematics library was created</a>**.

Numbers are represented as **strings** in this library, so they are not limited by the imprecision of **float**, but the operations performed are several orders of magnitude slower (but still faster than if we programmed such a function ourselves).

For example, if we want to get a square root from 2 to 3 decimal places, we just call:

```php
echo bcsqrt('2', 3); // 1.414
```
