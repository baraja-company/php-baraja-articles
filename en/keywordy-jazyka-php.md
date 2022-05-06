PHP Keywords
============

> id: d58620d7-57a0-410a-ba73-31a1f9d984fb
> slug:
> 	cs: keywordy-jazyka-php
> 	en: php-keywords
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '38fbdb53b4455903f3300c3c8b4a8ad2'

Almost all programming languages consist of `keywords`, which are special expressions of the language that have a special meaning.

An example of a keyword is the word `string`, `if` or `echo`.

It is important to note that keywords (sometimes also `commands`) <a href="/commands-and-functions">are not functions</a>, so `echo`, for example, is not a function either.

The list of keywords is good to know because they have special meanings in PHP and cannot always be used for everything. For example, the name of a class must not be the same as one of the existing keywords.

Example parse error
-------------------

When attempting to process a class named `String`, a `Parse error` occurs because PHP does not know if it is a class name or a data type:

This is wrong:

```php
class String
{
   // ...
}
```

List of keywords
-------------------

Updated keyword list for PHP 7.1:

`and`, `or`, `xor`, `for`, `do`, `while`, `foreach`, `as`, `return`, `die`, `exit`, `if`, `then`, `else`, `elseif`, `new`, `delete`, `try`, `throw`, `catch`, `finally`, `class`, `function`, `string`, `array`, `object`, `resource`, `var`, `bool`, `boolean`, `int`, `integer`, `float`, `double`, `real`, `string`, `array`, `global`, `const`, `static`, `public`, `private`, `protected`, `published`, `extends`, `switch`, `true`, `false`, `null`, `void`, `this`, `self`, `struct`, `char`, `signed`, `unsigned`, `short`, `long`
