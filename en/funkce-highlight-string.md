Highlighting PHP code syntax with highlight_string()
====================================================

> id: '72338b08-9e4d-45c0-a5d2-7f8ee705ae93'
> slug:
> 	cs: funkce-highlight-string
> 	en: highlighting-php-code-syntax-with-highlight-string
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '5a538abc-5464-4707-9211-ea86885c80d4'
> sourceContentHash: '3e6dc62a4808800affdf671ce6acc6df'

There are a number of libraries for highlighting PHP code that have varying reliability and are differently challenging to install.

If you don't have a lot of requirements for highlighting, you can also use the built-in PHP functions directly.

Basic usage
-----------------

The smallest possible example:

```php
highlight_string('<?php phpinfo(); ?>');
```

This function returns the highlighted code directly to the output using HTML tags (PHP 4):

```html
<code><font color="#000000">
<font color="#0000BB">&lt;?php phpinfo</font><font color="#007700">(); </font><font color="#0000BB">?&gt;</font>
</font>
</code>
```

In PHP 5, the output looks a bit better (it uses the `style` attribute correctly):

```html
<code><span style="color: #000000">
<span style="color: #0000BB">&lt;?php phpinfo</span><span style="color: #007700">(); </span><span style="color: #0000BB">?&gt;</span>
</span>
</code>
```

The code is automatically escaped, you don't have to worry about dumping it into the page. It does not contain XSS vulnerability.

How highlighting works internally
----------------------------------

The process of highlighting code is very complicated because the tool must understand the syntax of the language and know all its rules.

PHP solves this task by first breaking the code received as a string into a series of small logical units through a process called **tokenization of strings** (this can be used for many more things, such as <a href="/pokrocila-kalkulacka">implementing an advanced calculator</a>).

The individual tokens then get their own color and are printed in the same order as they were in the original source code.

> **WARNING:**
>
> Because the output buffer is used internally to output the value, the function cannot be used as a callback buffer function `ob_start()`.

Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$str` | `string` | *not* | PHP code to be highlighted. It should include the leading `<?php` tag. |
| `$return` | `bool` | null | If `true`, the function will return the highlighted code. |

Return values
----------------

If `$return` is set to `true`, the function returns the highlighted code. If `$return` is set to `false`, the function returns true on success and prints the highlighted code.

It returns `false` on failure.

Configuration
-------------

We can set custom colors for individual PHP tokens using the `ini_set()` function, for example as follows:

```php
ini_set("highlight.comment", "#008000");
ini_set("highlight.default", "#000000");
ini_set("highlight.html", "#808080");
ini_set("highlight.keyword", "#0000BB; font-weight: bold");
ini_set("highlight.string", "#DD0000");
```

Additional resources
------------

[Official highlight-string documentation]([Official manual](https://www.php.net/manual/en/function.highlight-string.php))
