How to understand PHP code
==========================

> id: d8d452da-37b4-4502-a573-a27afe7ea37a
> slug:
> 	cs: jak-se-vyznat
> 	en: how-to-understand-php-code
> 
> publicationDate: '2020-04-11 18:45:23'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c3012a19e6dd01770fad9ab456a5b6b3

Most languages can be written in different ways, with the same result. At the same time, once you have written the code, you will probably read it sometime in the future and correct it or add new features.

Therefore, to avoid having to think about the code all the time and to navigate it well, there is a set of tools and ways to "write code right" directly in PHP, or to build code in a way that directly supports its future readability (even by another human).

> **Author's note:**
>
> Experience shows that code becomes obsolete so fast that even the author of the application himself perceives his own code as foreign after half a year. So if we write it correctly from the beginning, it will not prevent its future extensibility.
>
> In real development, it is secretly shown that uniform code formatting and the introduction of rules in general prevents a number of errors.

TL;DR
-----

Code readability is often related to formatting and writing rules.

Within development teams, it makes sense to establish formal rules for how we format and maintain code.

Personally, I use (in 2022) the coding standard for the Nette framework and the rules are evaluated automatically in each commit. See the article on [using GitHub CI](https://php.baraja.cz/github-actions-nejlepsi-ci-pro-rok-2021#hotove-priklady) for more information.

Installing the coding standard test and running it is done with a pair of commands:

``shell
composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
php temp/coding-standard/ecs check src
```

Notes in the code
---------------

The notes have no effect on the processing of the code and are for the programmer's use only. For larger, more complete pieces of code, it is important to write a note that explains what the code is for and how it works in principle.

```php
<?php
// Variable definitions
$a = 5;
$b = 3;
$c = 2;

// Sum of all numbers
$sum = $a + $b + $c;

// Listing for user
echo $sum;
```

The note starts with a pair of slashes (`//`) and is valid until the end of the line. It can be used anywhere.

The note should not explain the specific implementation of the algorithm, but rather its general principles. This is because the code may change several times over time, in which case we should correct the note as well.

> **Author's note:**
>
> It is often the case that code does not do exactly what its description explains. This is mainly due to the programmer making a mistake somewhere. The note should therefore describe the general principles so that we can then modify the code accordingly. But never forget that the only truth about what actually happens in an application is described only by the actual code, and the note has no effect on that.

Graphical separation of code parts
----------------------------

When designing an application, it is important to separate the logical blocks from each other. Usually they are separated into functions, methods, or in the case of basic code at least by comments.

In a longer algorithm, I usually describe the whole principle of the algorithm at the beginning first, and then number the individual places in the code so that the developer can better understand the specific functionality based on them.

```php
/**
 * The function calculates the arithmetic mean.
 *
 * 1. Getting a list of numbers
 * 2. Getting the sum and number of numbers
 * 3. Calculate and print the average
 */

// 1.
$numbers = [1, 3, 8, 12];

// 2.
$sum = array_sum($numbers);
$count = count($numbers);

// 3.
echo 'The average is: ' . ($sum / $count);
```

The characters `/**` start a multi-line comment that applies up to the `*/` marker. To keep it easy to read, it's a good idea to put an asterisk at the beginning of each line.

Documentation comments
----------------------

Documentation comments are usually used to describe and document functions (behavior, parameters, return values, author, etc.).

In earlier versions of PHP (before `7.0`), data types were not yet used, so the type of a particular variable was described directly in the comment.

```php
/**
 * @author Jan Barášek <jan@barasek.com>
 * @license MIT
 * @link https://php.baraja.cz
 * @param float[] $numbers
 */
function average(array $numbers): float
{
    $sum = array_sum($numbers);
    $count = count($numbers);

    return $sum / $count;
}
```

Documentation comments are called `documentation` mainly because they have a prearranged format that is understood by specific development environments (and editors), but also by automated tools for generating documentation or checking code.

Write code in Czech or English?
-----------------------------

I write all code in English only (including function names, variables, comments, ...).

This has a number of advantages:

- The developer can proactively train his English right away.
- A large part of the application uses third-party libraries that are in English, so it automatically maintains consistency
- Most of the advanced stuff doesn't have English translation at all
- I'm sure you can think of many other examples

PHP doesn't directly require English and you can write everything in English. I see the use of English more as a kind of investment for the future, and an opportunity to easily extend the code by other people who are not native English speakers.

Completely localized code in English is also used in companies, so it's good to practice English right from the start.

Order of numerical operations
------------------------

Always keep in mind that PHP rounds numbers when performing numerical operations. This can often be a nuisance, as any result with decimal numbers is accompanied by some inaccuracy.

A good solution seems to be to first increment the numbers and then calculate with the largest possible numbers. In this way, there is statistically less distortion.

Example:

```php
echo 10 / 3; // Prints 3.3333333333333
```

In some cases, you can also use the trick of not using decimals at all and calculating everything as an integer. In this case, no such distortion will occur:

```php
echo 1 / 2 * 2; // this is worse because 1/2 = 0.5 * 2 = 1
echo 2 * 1 / 2; // this is better because 2*1 = 2/2 = 1
```

When you solve large complex numerical operations, use fractions to write numbers.
