Cycles and their types in PHP
=============================

> id: e2927790-d0fb-4de5-9848-01fdd088464b
> slug:
> 	cs: cykly
> 	en: cycles-and-their-types-in-php
> 
> perex: 'Cykl for, while, foreach a rekurze v PHP + vysvětlení rozdílů mezi cykly. Možnosti iterativního zpracování úloh.'
> publicationDate: '2020-04-11 18:56:34'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '297931d6e7b8c9e011dbbe06b414cdde'

Loops in general are used to repeat the same code section (usually over a data set) For each type of task, a different type of loop is suitable, which differs mainly in meaning and syntax. It is also important to note that all types of loops are convertible between each other, it just may not always be worthwhile in terms of code complexity and computation time.

The basic division of loops in terms of element type:

- `for`: A generic loop where we know the number of repetitions in advance, or can simply calculate it in advance,
- `while` and `until-while`: A general loop where we do not know the number of repetitions in advance,
- `foreach`: Browsing through arrays and objects,
- `recursion`: Decomposing a complex problem into a set of smaller ones.

General properties of loops
-----------------------

Cycles generally work by repeating a marked piece of code **over and over again**, **as long as the terminating condition** holds. The loop is stopped either by failing to satisfy the terminating condition, or by manually stopping it by calling `break;`.

If nothing stops the loop, then nothing prevents it from running indefinitely, or until the application is manually terminated, or the time limit for processing the PHP script runs out (usually 30 seconds).

**Important terms:**

- `Cycle` - a language expression that allows you to repeat a certain part of the code
- `Iteration` - one specific execution of the loop body
- `Initialization` - the start of the loop execution (before the first iteration begins)
- `Increment` - incrementing an iteration variable by one
- `Exit condition` - A condition that is verified at each iteration (the location of the verification varies by loop type). The loop runs as long as the condition holds

For loop
----------

The `For` is useful for cases where the number of repetitions is known in advance (or can be easily calculated). It is perfect for linear traversal of intervals, for example.

It is generally written (3 parts):

```php
for (initialization; expression; increment) {
    // loop body
}
```

- `initialization`: Definition of the initial state of the loop (what variables we need),
- `expression`: Condition. As long as it is valid, the loop will execute,
- `increment`: Change the state of the loop from the previous pass (`increment` - increment by one).

An example would be to output a series of numbers from `0` to `10`:

``php
for ($i = 0; $i <= 10; $i++) {
    echo $i . '<br>';
}
```

Or multiples of two from `2` to `100`:

```php
for ($i = 2; $i <= 100; $i += 2) {
    echo $i . '<br>';
}
```

The for loop comes in handy for various tricks, such as <a href="/abeceda-cisla-intervals">getting the alphabet, an array of numbers and intervals</a>.

While a do-while loop
-----------------------

A `While` loop runs as long as the condition holds. The difference between `while` and `do-while` is when the condition will be evaluated.

```php
while (expression) {
    // loop body
}
```

Alternatively:

```php
do {
    // loop body
} while(expression);
```

- The `while` first evaluates the condition and then runs the inner loop,
- `do-while` first processes the inner loop and then evaluates the condition.

This is particularly advantageous when we cannot know in advance if the loop will ever run and want to guarantee that it always runs at least once (then we use `do-while`).

Example:

> We have a number stored in the variable `$x = 2`. We want to also generate a number in the interval `<0; 10>` in the variable `$y` so that it is different than the number in the variable `$x`. Simply put: Generate a random number between `0` and `10` that is not `2`.

In this case, it is convenient to use a `do-while` loop. We don't know in advance how many times the loop will have to run (we may be unlucky and keep hitting the same number, in which case the loop will run for a long time), and we also want to guarantee that it will run at least once before the condition evaluates, since we have to generate the number first.

```php
$x = 2;
$y = $x;

do {
   $y = rand(0, 10);
} while($x == $y);

echo 'X : ' . $x . '<br>';
echo 'Y: ' . $y;
```

Foreach
-------

`foreach` is perfect for browsing fields and objects. We can get not only specific values, but also keys.

If we only want to get specific values:

```php
foreach ($data as $value) {
    // data processing
}
```

Alternatively, we can also get the keys:

```php
foreach ($data as $key => $value) {
    // data processing
}
```

The `foreach` is great for going through real data - like results from a database, or fields with keys:

```php
$countries = [
    'cz' => 'Czech Republic',
    'en' => 'Slovakia',
];

foreach ($countries as $shortcut => $name) {
    echo $shortcut . ': ' . $name . '<br>';
}
```

Recursion
-------

Recursion occurs when a function or method calls itself. It serves to break down a complex problem into a group of smaller ones.

> Understanding recursion can be challenging for beginners because it is based on the idea of passing responsibility.
>
> The function actually says something to the effect of "I can't solve this problem, but I know someone who can...", so it calls him, he calls someone, ... until the final member is called, and it cuts the problem.

It's certainly worth noting that any recursive algorithm can be rewritten to use loops where recursion is not needed (the reverse is also true). Recursion is a good servant, but a bad master. It helps to solve some types of problems simply and very efficiently, while looping through cycles is useful for other things.

In general, recursion is memory intensive because it creates new instances and contexts of functions and methods all the time. However, for traversing tree structures, for example, this is the only reasonable way to solve the problem.

Example:

We need to calculate the factorial of the number `5`, this is how it is done in mathematics:

`5! = 5 * 4 * 3 * 2 * 1`

That is, there is a successive multiplication of a series of numbers from `1` up to the number we are interested in the factorial of.

Recursively, this can be solved very elegantly:

```php
function factorial(int $n): int
{
   if ($n === 0) {
      return 1;
   }

   return $n * factorial($n - 1);
}
```

Alternatively, an even shorter implementation using the ternary operator:

```php
function factorial(int $n): int
{
    return $n === 0 ? 1 : $n * factorial($n - 1);
}
```

We can rewrite the same thing to use loops:

```php
function factorial(int $n): int
{
    $result = 1;

    for ($i = $n; $i > 0; $i--) {
        $result *= $i;
    }

    return $result;
 }
```

Writing with a loop is a bit longer, but it is significantly less memory intensive. We use only one variable `$result` for the calculation, where we change the value continuously and do not have to keep creating new instances of `factorial()`.

Write infinite loops very carefully
-----------------------------------

It can be very easy for a loop to become infinite (we achieve this by never satisfying the termination condition). You should take this into account and possibly stop the loop yourself with `break;`.

For example, roll the die until the number `6` is rolled. The implementation tempts you to use the `while` loop:

``php
while (true) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'The value has fallen ' . $value;
      break;
   }
}
```

By writing `while(true)` we have ensured an infinite number of repetitions, because `true` will always be true. So we have to stop the loop ourselves with the `break;` construct, but what if the value `6` never falls and we never stop the loop?

Personally, I always count the number of repetitions and if it exceeds some critical limit, I forcibly terminate the loop. For example, if we exceed `1000` attempts. In that case, it's better to use `for` rather than `while` and count the number of runs.

If we exceed the number of runs, it is polite to inform the developer by throwing an exception.

```php
for ($i = 0; $i <= 1000; $i++) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'The value has fallen ' . $value;
      break;
   }
}

if ($i === 1000) {
   throw new \Exception('The maximum number of throws has been exceeded.');
}
```
