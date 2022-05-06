Special meaning of quotation marks
==================================

> id: '2649942d-94d6-48c3-9c78-5a303165bf72'
> slug:
> 	cs: uvozovky-vyznam
> 	en: special-meaning-of-quotation-marks
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c1b63b98a3e9d9c642247c363747616a

In PHP, you can often replace quotation marks with apostrophes and achieve the same effect. Sometimes we even use a combination of quotes and apostrophes on purpose to achieve different output without having to use escaping.

**TLDR: Using quotes can be dangerous in some contexts, so use apostrophes everywhere instead. Quotation marks are only suitable for special cases.**

| Character | Name |
|------|-----------
| `"` | Quotation mark |
| `'` | Apostrophe |

Example one - same output
-----------------------------

```php
echo "hello world!"; // this is the same
echo 'hello world!'; // like this
```

In this case, the use of quotes or apostrophes doesn't matter. So, on the surface, it may look like there is no difference between quotes and apostrophes.

Combination of quotation marks and apostrophes
------------------------------

Consider the following code:

```php
echo 'My favorite color is "red"!';
```

If I used `carriage returns` to delimit the string being output, it would **be ambiguous** where the string **begins** and where it ends, so I used `apostrophes` to delimit the string, which solves this problem.

Inserting quotation marks into a string delimited by quotation marks
---------------------------------------------------

Occasionally, there may be situations where I need to list both `quote` and `apostrophe` within the same string. You can use not only concatenation of two strings, but also so-called **escaping** of characters.

```php
echo "This string \"contains\" quotes.";
```

The backslash in this case means "exactly this character" and therefore the quotation mark will not be interpreted as the end of the string (or anything else) and therefore it will always be a quotation mark. Other strange characters can be marked in this way where their durability cannot be guaranteed and the correct meaning might be misunderstood.

Variable within a string
-----------------------

Quotation marks can be extremely tricky because they allow you to insert variables directly into a string:

```php
$color = 'red';

echo "My favorite color is {$color}, and yours?
```

Much safer are apostrophes, which don't allow this and you need to collapse the string:

```php
$color = 'red';

echo 'My favourite colour is ' . $color . ', and yours?';
```

Good habits
--------------------------

In general, I recommend using apostrophes (if possible) to delimit anything, as they are much less common in strings than just quotation marks.

Moreover, PHP is a web language, i.e. it is used to generate HTML documents, where quotation marks are very common precisely because they are also used to generate parts of HTML code. Personally, I recommend getting used to strictly using apostrophes everywhere, because then you don't have to remember what you're enclosing.

Quotation mark behaviour
--------------------------

Watch out! Don't throw away the quotation marks completely! They have some special advantages that can be useful for advanced PHP work - however, beginners consider them as errors and don't understand them.

```php
$x = 10; // set a variable
echo "The value of the variable is: $x, exactly."; // and the output
```

Within the quotes, you can directly list the values of the variables, or the dollar sign causes everything after it to be a variable. So if you don't want to output the value of the variable, but the dollar sign, you have to escaped.

```php
$price = 25; // price in dollars
echo "Product price: $price$"; // prints "Product price: $25"
```

The benefit of quotes is questionable in this case, and it may be better to use apostrophes and simply concatenate the strings.

```php
$price = 25;
echo 'Product price: ' . $price . '$'; // prints the same as the previous example
```

> **Note:** The price in dollars is correctly written in the format `$25`, which would make it even more confusing because writing `$$price` actually calls something called a `variable variable` (in the variable name, we say the name of the variable we're going to call - just confusing).

**TIP:** You might be interested in <a href="/promenna-variable">variable variable</a>.

String designation
--------------------------

In general, anything in quotes or apostrophes is treated as a string. Thus:

```php
$x = 5; // this is something else,
$x = "5"; // than this.
```

In the first case, the **number** 5 is stored in the **$x** variable; in the second case, the **string** "5" is stored in the same variable. Fortunately, this doesn't matter in PHP, and you can work (almost) the same way with either variant, because PHP can automatically **retype** variables according to their contents. However, it's generally recommended not to write numbers in quotes, especially for computational operations where rounding errors can then occur.

Sometimes I may want to store the output of a function in a variable:

```php
$pi = 3.14159;
$round = round($pi); // this is correct
$round($pi) = "round($pi)"; // this is "wrong" (the question is what output I expect).
```

The error in the second case is just in the quotes, where the output of the **round()** function is not stored in the **$round** variable, but the string calling that function.
> **TIP:** The value `$pi` does not have to be entered directly into the script like this and we can use the `pi()` function, which is more accurate for more complex calculations.

Special meaning of escaping
--------------------------

> **Warning:** The following examples only work in quotes, if you enclose them in apostrophes they will behave like regular characters without the special meaning (except `\'` which escapes the apostrophe).
Escaping is for when I want to output some special character inside quotes or an apostrophe that could be interpreted as a language expression and therefore processed, even if the programmer didn't intend it. We have already shown an example; this section describes possible behavioral exceptions.

Indeed, sometimes escaping itself carries a special meaning. Example:

```php
echo "Long text, separated by two lines.";
```

The previous example prints:

```php
Long text, split into
two lines.
```

So if we want to output a slash, we must also escaped it (escaping the `n` character is not necessary in this case, because it would again be understood as a wrap, or in this case we must not escaped at all):

```php
echo "Long text, separated by two lines.";
```

There are several similar special characters, for example `\t` will do a tab. The full list is in the official documentation.

Real use of special characters in escaping
-----------------------------------------------

To begin with, I would like to point out that almost anything can be done in PHP in multiple ways, and the examples given here are just to illustrate how the problem can be approached.

For example, if we want to parse text line by line, we can use the <a href="/explode">explode</a> function.

```php
$parser = explode("\n", $retezec); // parses the text line by line
```

In this case, using the special character `\n` makes sense, because we can very effectively say that we want to parse by line breaks.

```php
$parser = explode('
', $string);
```

> **Warning:** On Unix and Windows systems, there is some confusion about the characters used to indicate line breaks. For example, Windows uses `CRLF` (a pair of `\r\n` characters), while Linux uses only `LF` (a single `\n` character). When parsing by line, this should be kept in mind. Usually the problem is solved by normalizing the characters to `LF` only.

Summary
-------

If you can, use **apostrophes** everywhere.

It's good to know the use of quotation marks and only use them where necessary (or generally good). If you are outputting text that may contain quotes, enclose it in apostrophes (which then behave more predictably). Personally, I use quotes to express various special characters that are unnecessarily complex to enter in apostrophes and would require complex escaping.
