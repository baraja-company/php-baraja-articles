Regular Expressions in PHP
==========================

> id: '32142161-6ace-4cfd-b472-b48031219e9b'
> slug:
> 	cs: regex
> 	en: regular-expressions-in-php
> 
> perex: 'Regulární výrazy a jejich kompletní vysvětlení. Hledání podřetězce, pokročilé nahrazování a generování řetězců.'
> publicationDate: '2020-03-08 13:37:38'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '392c8f14e6943dd8f345a9a134e0de92'

Regular expressions are tools that allow you to easily search, validate, compare, split, collapse, and replace strings according to a mask (pattern). It is a very powerful and elegant tool for advanced string manipulation.

Mask
-----

At the beginning, we first need to come up with the actual regular expression that we are going to execute. It is entered as a text string, which has a bunch of rules and configuration options (this is a very complex technique).

For starters, it's important to note that the regular expression is evaluated sequentially from the left to the right, and if there are multiple ways to interpret the string, the largest possible match is always used (it behaves in a **hungry** fashion, trying to process as many characters as possible).

The behaviour and processing strategy of a regular expression can be influenced by switches, of which there are many.

Simple verification that the string is a valid e-mail
-----------------------------------------------

How can we simply check that the string `jan@barasek.com` is a valid email address without having to split it into complex parts or go through it character by character?

Regular expressions give the answer (the above expression is very simplified for the purposes of the example, and a real implementation of email address validation should be a bit more complicated):

```php
$mail = 'jan@barasek.com';
$regex = '/^.+@.+.(en|en|com)$/';

if (preg_match($regex, $mail)) {
   echo 'Email is valid';
} else {
   echo 'Email is invalid';
}
```

Let's examine the expression `/^.+@.+\.(en|en|com)$/` in a little more detail:

First, we need to wrap the entire expression in a pair of `/` characters (at the beginning and at the end) that tell where the expression starts and ends. The `/` at the end of the expression is followed by any modifiers (expression processing mode settings).

When processing an expression, you proceed from the left side character by character. Each has its own meaning, which is given in the following table:

| Character | Meaning | Description | Example |
|------|-----------------|-------|-------------
| `^` | Start of string | Forces that the string must start at this point. | Forces that the string must start with the sequence `+420` (useful for number validation, for example): `/^+420/`. |
| `$` | End of string or line | Forces that the string or line must end here. The end of line is then asserted with `\z`. <a href="https://phpfashion.com/vite-co-znamena-v-regularnim-vyrazu">Detailed explanation</a>. | The file name must be a text file (ending with a period and then the string "txt"): `/\.txt$/`. |
| `.` | Any character | Catches absolutely any character. | Verifies that the string contains exactly one of any character: `/^.$/`. |
| `\d` | Number | Detects characters `0-9` | Detects a phone number that contains no spaces and has 9 digits: `/^(\+420)?\d{9}$/`. |
| `\s` | Whitespace | Catch spaces, hyphens and tabs. | Allows spaces between digits in a phone number in triple digits: `/^(\d{3}\s?){3}$/`. |
| `+` | Multiple characters, but at least one | Repeats the previous subexpression and tries to catch as much as possible. The subexpression must be repeated at least once. | Catches as many digits as possible, but at least one: `/\d+/`. |
| `*` | Multiple characters, can be none | Works the same as `+`, but allows to catch an empty string (the value does not have to be present). | Catches as many digits as possible, if none exist, catches an empty string: `/\d*/`. |
| `(` `)` | Brackets | Indicates a subexpression. This can be used to enclose several different tags and then require, for example, repetition over them, or to trap their contents in a variable. | Let's divide the postal code into 2 parts according to the space, which is optional and there can even be more than one: `/^(\d{3})\s*(\d{2})$/` |
| `\|` | Or | Contains a subexpression, or another subexpression. | Number starting with `+420` or `+421`: `/^+(420\|421)\s*\d+$/`. |
| `\.` | Escaping | If we want to catch a character in an expression that otherwise has a special meaning, we need to escaped it in the same way as, for example, strings in PHP. | Catches a pair of numbers separated by a period (if we didn't escaped the period, it would be understood as "any character"): `/\d+\.\d+/`. |

Just for completeness, I'll give the complete form of the validation rule for email as implemented by Nette:

```php
/**
 * Finds whether a string is a valid email address.
 */
public static function isEmail(string $value): bool
{
   $atom = "[-a-z0-9!#$%&'*+/=?^_`{|}~]"; // RFC 5322 unquoted characters in local-part
   $alpha = "a-z\x80-\xFF"; // superset of IDN
   return (bool) preg_match("(^
      (\"([ !#-[\]-~]*|\\\\[ -~])+\"|$atom+(\$atom+)*) # quoted or unquoted
      @
      ([0-9$alpha]([-0-9$alpha]{0.61}[0-9$alpha]))+ # domain - RFC 1034
      [$alpha]([-0-9$alpha]{0,17}[$alpha])?                # top domain
   \\z)ix", $value);
}
```

`preg_match()` - validation by pattern
------------------------------------

The basic function for format validation and parsing is `preg_match()`, it has 2 mandatory parameters and the third can be used to specify the output field.

Example:

```php
$psc = '272 01'; // Kladno

if (preg_match('/^(\d{3})\s*(\d{2})$/', $psc, $parser)) {
   echo 'The postcode is valid [' . $parser[1] . ', '. $parser[2] . ']';
} else {
   echo 'ZIP code is invalid';
}
```

The code will return `Zip code is valid [272, 01]`.

Note the single parentheses, which we used to split the expression into several smaller parts. This then makes it possible to get the individual subexpressions as array entries. The whole function then returns `true` or `false` depending on whether the string was successfully captured.

Sometimes, however, navigating the numerical order of the parentheses is very challenging, as the number may change, or there may simply be too many of them. In this case, it is possible to name the brackets individually and then access the keys using their names.

For example:

```php
$phone = '777 123 456';

preg_match('/^(?<operator>\d{3})\s*(?<number>[0-9 ]+)$/', $phone, $parser);

echo $parser['operator']; // returns 777
```

`preg_replace()` - replace by pattern
----------------------------------------

It is also possible to replace strings using regex, which is particularly useful for various post-user format corrections.

Suppose we want to store a user-entered phone number in an integer, since this is required by a third-party library, but users can enter it in some pretty wild formats.

In that case, I stick to the dictum:

> "Be generous in what you receive and strict in what you send".

That's why we automatically adjust the format. First we use parsing to break the string into its individual parts, and then we fold it back according to the bracket numbers:

```php
function formatPhoneNumber(string $phoneNumber): int
{
   return (int) preg_replace(
      '/^(\+\d{3})\s*(\d{3})\s*(\d{3})\s*(\d{3})$/',
      '$2$3$4',
      $phoneNumber
   );
}

echo formatPhoneNumber('+420 777 123 456');
```

Constructing a string according to a regular expression
----------------------------------------

Regexes also make great sense when generating new strings according to a complex pattern.

Pure PHP has no support for this, but we can download a third-party <a href="https://github.com/icomefromthenet/ReverseRegex">ReverseRegex</a> library that can do this.

For example, we may want to generate a set of passwords based on the regex `[a-z]{10}` and nothing will stop us:

```
jmceohykoa
aclohnotga
jqegzuklcv
ixdbpbgpkl
kcyrxqqfyw
jcxsjrtrqb
kvaczmawlz
itwrowxfxh
auinmymonl
dujyzuhoag
vaygybwkfm
```

The usage is as follows:

```php
use ReverseRegex\Lexer;
use ReverseRegex\Random\SimpleRandom;
use ReverseRegex\Parser;
use ReverseRegex\Generator\Scope;

require 'vendor/autoload.php';

$lexer = new Lexer('[a-z]{10}');
$gen = new SimpleRandom(10007);
$result = '';

$parser = new Parser($lexer, new Scope(), new Scope());
$parser->parse()->getResult()->generate($result, $gen);

echo $result;
```

I generate my math examples in Nette in Presenter this way, for example, and it's possible with real ease:

```php
public function actionRegex(): void
{
   $lexer = new Lexer('\d{1,3}[\+\-\*\/]');
   $parser = new Parser($lexer, new Scope(), new Scope());
   for ($i = 0; $i <= 10; $i++) {
      $result = '';
      $gen = new SimpleRandom($i);
      $parser->parse()->getResult()->generate($result, $gen);
      dump($result);
   }
   $this->terminate();
}
```

The important thing for the library is that it still generates the same output for the same input (even though it might seem that there are many possible strings to match for each regular expression). If we want to change the generated expression randomly, we also need to change the `seed` by which the output string is generated. Either traversing the seed interval or perhaps the `rand(1, 1e6)` function is useful for this.

Error catching and processing
-----------------------------

In PHP, catching errors in regexes is pretty hell, but there is still a solution.

This is explained in detail in the article <a href="https://phpfashion.com/zradne-regularni-vyrazy-v-php">Treachable regular expressions in PHP</a> by David Grudel.
