Escaping characters in a string in PHP
======================================

> id: '40f9361e-e286-4b5e-a0c0-1f6cda8106af'
> slug:
> 	cs: escapovani
> 	en: escaping-characters-in-a-string-in-php
> 
> publicationDate: '2019-11-26 11:56:52'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '026284252541bc9c918a87298b9e261c'

Escaping is used to write characters that have different meanings in different contexts.

For example, we want to insert another quotation mark into a string enclosed by quotation marks. How to do it?

There are 2 options:

```php
echo "Levi's jeans"; // Combination of quotation types

echo 'Levi's jeans'; // Escaping with backslash
```

Escaping is also important to do when outputting variables to an HTML template, where the contents of the string may be in a different context and mean something special.

Therefore, for example, when listing HTML code (which we have in a variable) we need to treat the listing, otherwise the HTML code will execute.

For example:

```php
$message = 'Hi <b>Thomas!</b>';

echo $message; // Wrong!

echo htmlspecialchars($message); // Right :)
```

The issue of escaping is very complex and I recommend reading the article <a href="https://phpfashion.com/escapovani-definitivni-prirucka">Escaping - The Definitive Guide</a> by David Grudel.
