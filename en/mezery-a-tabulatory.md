Indent code using spaces and tabs
=================================

> id: '116f19ed-3753-498d-bb9e-e0f93b88c347'
> slug:
> 	cs: mezery-a-tabulatory
> 	en: indent-code-using-spaces-and-tabs
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c3af36d7df67cf36b940c9702cb71549

To keep the code easy to read for other programmers and to keep it elegant, we need to learn to format it uniformly. This article discusses the use of spaces and tabs.

Are spaces or tabs better for indenting code? This is often an endless topic for debate, if you're looking for a quick and unambiguous answer, most good programmers prefer to use tabs, but let's break it down nicely.

Spaces
----------------------

Every programmer and editor uses a different amount of spaces for indentation (but most often 4), which leads to inconsistent code that can be harder to read when reading someone else's code. Additionally, more characters are needed for indentation (which increases its data size).

However, spaces have an advantage when rendering code in a web browser (where the HTML entity `&nbsp;` is used for indentation), so it is a relatively easy to portable format that only gains an advantage as a stable and reliable rendering method (4 spaces will always appear as 4 spaces).

Tabulators
----------------------

They are whatever width the programmer sets in the editor (if the editor can do that), so if you like a particular indentation, no problem - we can each look at the same code with different tab widths. At the same time, it's a very economical character that doesn't need to be repeated as often as just spaces.

When rendering tab-indented code into an HTML page, it is customary to replace tabs with fixed spaces to ensure correct display in all browsers:


```php
$code = '<?php
    $a = 5+3;
    $b = 4;
    if ($a > $b) {
        echo $a . " > " . $b;
    } else {
        echo $b . " <= " . $a;
    }
?>';

echo str_replace('\t', '&nbsp;', $code);
```
