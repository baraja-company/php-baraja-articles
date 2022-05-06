Ternary operators in PHP (?:) - condition in one line
=====================================================

> id: '1191be9b-ff9d-4725-b0b4-17b60de2b935'
> slug:
> 	cs: ternarni-operator
> 	en: ternary-operators-in-php---condition-in-one-line
> 
> perex: Ternární operátor umožňuje zapsat jednoduchou podmínku do jednoho řádku jako výraz.
> publicationDate: '2019-11-26 11:59:18'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: '491dbbbaeceba356a030e2501f5c0e2b'

The ternary operator allows you to shorten a simple condition to a single line in a place where parsing is unnecessary, complex, or outright inappropriate.

TL;DR
------

The ternary operators are shorthand for `if` and `else` conditions, so you don't need to use them. They are useful for ever repeating pieces of verification logic. Always use the format `(condition ? positive logic : negative logic)` including parentheses. Use for short verification to make the code clearer and shorter.

Basic definitions
------------------

Often we have a condition of the form, for example:

```php
$a = 5;
$b = 3;

if ($a > $b) {
     echo 'It's bigger';
} else {
     echo 'It is smaller';
}
```

So if we want to output just one simple sentence, we have to use 4 lines of code unnecessarily, which could be reduced to one.

```php
$a = 5;
$b = 3;

echo 'Is ' . ($a > $b ? 'larger' : 'smaller');
```

In general, the ternary operator is written using 3 parts (that's why it's called "ternary"):

```php
(condition ? if true : if false)
```

Ternary operators are used really often in practice, for example to denote even rows in a table:

```php
$field = [3, 1, 4, 1, 5, 9, 2];

for ($i = 0; $pole[$i]; $i++) {
     echo '<tr class="' . ($i % 2 ? 'odd' : 'even') . '">';

          // Somehow working with a table here...
          // for example: echo '<td>' . $field[$i] . '</td>';

     echo '</tr>';
}
```

Example of using the ternary operator
------------------------------------

Quite often we need to check the existence of a variable value and use it immediately if necessary. If it doesn't exist, we want to return the default value.

The classical approach is to do this like this:

```php
$a = 5;
$b = 8;

$c = $a ? $a : $b;
```

If the variable `$a` exists, use `$a` as the value, otherwise use `$b`.

Sometimes, however, we get the value from a function:

```php
$a = 5;
$b = 3;
$default = 42;

$c = function($a, $b) ? function($a, $b) : $default;
```

This method of calling is very inefficient with respect to system resources. This is because the function must be called first, and if it exists, it is called again to get the value, which is stored in the variable `$c`.

This could be better handled via a helper variable:

```php
$a = 5;
$b = 3;
$auxiliaryVariable = function($a, $b);
$default = 42;

$c = $helpfulVariable ? $helpfulVariable : $default;
```

Inappropriate use
------------------

The ternary operator is not always worth using because it tends to cause confusion when writing complicated and nested conditions.

See for yourself an example:

```php
$valid = true;
$lang = 'french';

$x = $valid
     ? ($lang === 'french' ? 'oui' : 'yes')
     : ($lang === 'french' ? 'non' : 'no');
```

Would you know at a glance that the variable `$x` would contain the value `oui`?

After a little practice, you might, but that's not the right answer. :)

Verifying the (non-)existence of the value and using the default
--------------------

The ternary operators are most powerful in routine verification of (non-)existence of values and use of other defaults.

For example, we want to check if a main category exists for an article and if not, output a replacement message:

```php
echo $mainCategory ?? 'Category does not exist';
```

The operator `??` (two question marks) checks if the variable `$mainCategory` exists and is not `null`. It works in the same way as the `isset()` function.

Validating the emptiness of a value
-----------------------------

A very often useful construct for verifying that the output value is not empty (i.e. not `null`, `0`, `false` or `''*(empty string)*).

This can be written as follows:

```php
$a = 5;
$b = 3;
$default = 42;

$c = function($a, $b) ?: $default;
```

First `function($a, $b)` is called, then its value is tested and if it is not empty, it is immediately passed to the `$c` variable, otherwise the `$default` variable is used.

The `?:` operator works like the `!=` operator in a conditional (false regardless of data type), and can be remembered by looking like a smiley face with Elvis hair, for example.

Support for older versions of PHP
----------------------------

The `?:` operator has been working since PHP 7. In older versions, we have to make do with the `if (...)` condition, which can achieve the same behavior. Remember that ternary operators are really just a shortcut to write the same thing that is handled by conditions.

Non-existence of a value can be checked with the `isset()` function, which checks that the variable exists and is not empty (`null`).

Instead of code:

```php
$a = 5;
$b = 3;

$c = $a ?? $b;
```

Then we write the older alternative:

```php
$a = 5;
$b = 3;

$c = isset($a) && $a ?? $b;
```

> **Warning:** The order of `isset()` and the variable itself matters. If we reversed the order and the variable didn't exist, it would raise an access error on the non-existent variable.
