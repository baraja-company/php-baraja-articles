Conditions in PHP - IF() {...} - branching options
==================================================

> id: '3e4b81bb-a8bb-4e99-8842-e76f1658a371'
> slug:
> 	cs: if
> 	en: conditions-in-php---if-...---branching-options
> 
> perex: 'Podmínky, logické výrazy, booleovská logika a možnosti větvení PHP scriptů. Vyhodnocování logických výrazů a operátory. Možnosti skládání výrazu.'
> publicationDate: '2019-11-26 11:55:09'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '759a244b4fe2dc119c475aee42259578'

> **Note:** This article may be a bit messy for some beginners as it assumes a basic understanding of PHP. If you're interested in how conditions work, read <a href="/conditions">about conditions in the beginner's course</a>.

| Support: | All versions: PHP 4, PHP 5, PHP 7
|-------------------|----------|
| Brief description: | Validation of one or more statements
| Type: | <a href="/statements-and-functions">Statement, construct</a> (not a function)

Description
-----

Often you need to determine if an equality holds or if a statement is true, this is what conditions are for. PHP uses the following syntax like many other languages (especially C):

```php
if (logical statement) {
    // construct
}
```

Each Boolean expression has a value of `TRUE` (true) or `FALSE` (false), there are no other options.

Example of comparing whether the variable `$x` is greater than the variable `$y`:

``php
$x = 10;
$y = 5;

if ($x > $y) {
    // This part of the script will run if the condition holds
} else {
    // This part of the script executes if the condition does not hold
}
```

The condition construct has a mandatory content in round brackets that specifies the expression to be tested, consisting of operators (see below for an overview), multiple expressions can be linked using Boolean operators (see below for an overview).

In addition, the condition contains two optional statement blocks.

- If the condition holds
- If the condition does not hold

For practical reasons, always include at least the first block of statements when the condition holds, otherwise testing the expression would not make sense.

Use of semicolons and parentheses
--------------------------

In general:
- **Round** parentheses are used to separate logical expressions (other round brackets can be plunged to achieve more complex expressions).
- The **complex** bracket is used to delimit a block of commands and functions.
- The **middle** is not used to indicate a condition (the block of commands is delimited by a compound bracket), but to separate the individual commands within the condition).

The only possible notation with semicolon (except for the use of the **endif** construct):

```php
if ($x > $y);
```


However, such a condition is meaningless because in both cases the result of the comparison will be discarded and no statement belonging to the condition will be executed.

Alternative notation
--------------------------

Under certain circumstances, the `if` construct can be used with the omission of the compound brackets. This can only be achieved in the following cases:

- If we execute only one statement in the condition.
- If we use a colon and an endif instead of compound brackets;
- If we use "in-line" notation.

For more detailed information, see the following 3 chapters.

> **`1. Only one command ~ abbreviated syntax`**

If you are creating a condition in which you want to execute only one construct (statement), you can use either the classic compound bracket notation:

```php
if ($x > 10) { $y = $x; }
```


Or we can omit the parentheses:

```php
if ($x > 10) $y = $x;
```


However, this behavior only applies to one statement in the immediate vicinity of the condition.

A better example (only the `$y = $x` construct is executed conditionally, the rest is always executed):

```php
$x = 5;
$y = 3;
$z = 10;
if ($x > $y)
$y = $x;
$x = 3;
```

> **`2. Colon and endif;`**

```php
if (expression):
    construct;
    construct;
    construct;
endif;
```

However, this notation has long been considered obsolete because it reduces in orientation when multiple conditions are immersed in themselves.

> **Note:** I would like to note that this style is also liked by some people, such as Yuh (<a href="https://www.jakpsatweb.cz/php/php-tahak.html#vetveni">see his article</a> for more information). God forbid you use this somewhere.

> **`3. Ternary expression ~ single line "in-line" notation`**

Occasionally it is useful to do a simple in-line comparison with some other action (for example, along with defining a new variable). If we want to execute only one statement, the whole procedure can be reduced to a single line even while keeping it as simple as possible.

```php
$x = 5;
$xJeVetsiNotTwo = ($x > 2 ? TRUE : FALSE);

// or even shorter:

$xJeVetsiNezDva = ($x > 2);
```

Operator tables
-----------------

Two types of operators are used within the condition:

- **Comparative** ~ they compare a particular relationship,
- **Logical** ~ combine multiple expressions to create complex conditions.

Comparative operators
-----------------------

| Operator | Meaning |
|----------|----------|
| `==` | Equals
| `===` | Equals and has the same data type
| `!=` | Does not equal
| `>=` | Equals or is greater
| `<=` | Equals or is less
| `>` | Greater
| `<` | Less

Example (valid when `$x is not 5`):

```php
if ($x != 5) { ... }
```

Logical operators
--------------------

| Operator | Alternative | Meaning | True when:
|-----------|--------------|----------------|--------------------------------------------
| `&&` | AND | and at the same time | both values are true
| `||` | OR | or | at least one value is true
| `^^` | XOR | exclusive OR | at least one is true or false, but never both
| `!` | *doesn't* | negation of expression | `true` when `false` and vice versa
| `()` | *does not* | expression negation| depends on the circumstances

A more complex example:

``php
$x = 5;
$y = 3;
$z = 8;
if ($x > 0 && !($y != 2 && $z == $x) || $z > $y) { ... }
```


Omitting logical and comparison operators
---------------------------------------------

Often we can afford to omit either operator (or even both), but we must never forget the rules of proper usage to make the resulting expression work.

In general, when testing an expression without an operator, we test whether its value is `TRUE` or non-empty (for example, it contains a non-zero number, a non-empty string, ...).

Examples:

```php
$x = 5;
$y = 3;
$z = 8;

if ($x) { ... } // passes because $x is not empty
if ($x && $y) { ... } // passes because $x and $y are not empty
if (!$x) { ... } // fails because TRUE is negated
if (isset($z)) { ... } // passes because the variable $z exists
```

But tricky situations can arise, especially when:
- I ask for `if ($x)` and the variable `$x` contains zero (`0`), then the condition is not satisfied.
- Or the variable `$x` contains the string ``0`` (the number zero), because it overflows to zero and the expression is therefore not true.
- There is a function in the condition that always returns some non-empty string, because then the condition always holds.
- Or if we are checking input from the user and he returns `'false'` as a string, then again the condition is true because the string is non-empty.

I recommend one simple and effective solution to this - ask for the number of characters that are returned. If the string is empty (or the variable does not exist), then zero characters are returned and the condition is not satisfied. Simple example:

```php
$x = '0';
if ($x) { ... } // the condition normally does not apply
if (strlen($x)) { ... } // // the condition holds because $x contains 1 character
```

Next, we can test for the existence of a variable with the `isset()` function.

String comparison
-----------------

Finding out that the strings are identical is easy:

```php
$a = 'Cat';
$b = 'cat';

if ($a === $b) {
    // If the strings are the same
} else {
    // If the strings are different
}
```


It is important to keep a proper eye on the data types in case the entry might be equivalent to some other one.

For example, the empty string `$a = '';` is different from the string `NULL`: `$b = NULL;`. We need to make this distinction, for example, because of databases where there is a difference between a value not existing or being empty.

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

Also, when comparing strings, it is a good idea to ignore white (invisible) characters such as spaces, tabs, and line breaks. This is useful, for example, when entering a password and passing it to a hash function:

```php
$password = '81dc9bdb52d04dc20036dbd8313ed055'; // 1234
$userPassword = ' 1234 ';

if (md5(trim($userPassword)) === $password) {
    // The trim() function will automatically delete spaces.
}
```

Unknown (non-existent) value?
--------------------------

Sometimes it can happen that the value does not exist (it is neither `TRUE` nor `FALSE`), it is mainly a value obtained from the database (for example, we are asking for a column that does not exist), in this case the data type `NULL` will be returned.

In general, `NULL` is evaluated as `FALSE`, i.e. the condition does not apply. However, this behavior is not always convenient, because a non-existent value does not necessarily mean that there is no record.

> Example from practice: We have a user profile and we query the user's web page. Not all users need to have a web page, so in this case `NULL` is returned, but the user still exists. So in this case, we should rather use the `isset()` function to test for the (non-)existence of the variable and not make a conclusion based on a specific value.
