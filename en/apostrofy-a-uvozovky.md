Apostrophes and quotation marks
===============================

> id: '526ad995-3412-446e-bb56-9627dff8e29e'
> slug:
> 	cs: apostrofy-a-uvozovky
> 	en: apostrophes-and-quotation-marks
> 
> perex: Použití uvozovek a apostrofů pro ohraničení řetězců v PHP. Escapování řetězců a řešení kontextů.
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c5b0bf8b74238134be5348f886591e2a

You can use either quotes or apostrophes to delimit strings. I personally prefer only **apostrophes** unless it is a special line break character or tab.

There are a number of reasons for this, let's go through them constructively.

> The main reason not to use quotes is security and inappropriate handling of data types.

Using HTML tags inside a string
--------------------------------

If we need to return HTML code in a string, and we wrap the string in quotes, we have to escaping quite awkwardly:

```php
return "<a href="{$link}">{$text}</a>";
```

Or do the same thing in a more readable way, preserving the quotes without escaping:

```php
return '<a href="' . $link . '">' . $text . '</a>';
```

Longer visual distance of the string from the variable
---------------------------------------------

There's nothing worse than sticking variables inside a string directly on top of each other, where you can't quickly visually tell what's a variable and what's a string:

```php
$url = "{$baseImageUrl}/{$dirName}/{$basename}.{$ext}";
```

This notation has no negative effect on functionality, just the individual variables and fixed characters from the string are stacked too close together, which degrades readability.

Let's try again and make it more readable:

```php
$url = $baseImageUrl . '/' . $dirName . '/' . $basename . '.' . $ext;
```

The main advantage I see is the cleanliness around the variables, with no unnecessary characters in the name.

In addition, it will also be easier to wrap and there will be no wrapping characters in the string ;)

```php
$url = $baseImageUrl
       . '/' . $dirName
       . '/' . $basename . '.' . $ext;
```

It is not possible to call a function inside quotes
---------------------------------------

But a variable can, which is why "something" is appended outside the string (especially functions), but variables are written back inside. And sometimes the programmer even appends the variable after the string. In short, confusion over confusion.

Why can't we do things uniformly?

Inappropriate mapping of another data type to a string
---------------------------------------------------

Consider the following function call:

```php
echo getFullName("{$user->name}");

function getFullName(string $name): string
{
	// ... implementation ...
}
```

Variables can be inserted into quotes, which causes them to be rewritten to a string. However, if the variable is in a string itself and is of a different data type than string, the information about the original data type may be corrupted. For example, if the `$user->name` construct returned `false` or `null`, we wouldn't be able to tell that it was an error.

An application should always recognize an error condition and fail properly, rather than ignoring it. Proper error reporting leads to better debugging in the future.

Occasionally, you may encounter overrides, especially because a function requires a particular data type:

```php
trim("{$returnText}");
```

In that case, I'm leaning more towards write:

```php
trim((string) $returnText);
```

which doesn't look so "magical" and it is obvious even to less skilled users what happened to the variable.

Dropping the value `null` from the database
----------------------------------

Suppose we are retrieving the name of a hotel from a database:

```php
return "{$row->hotel->name}";
```

But what if the name doesn't exist and is `null`? By using quotes, we've created a potentially hard-to-detect error, because it will be rewritten to an empty string. However, we would like to return a real `null`. It makes a difference if the record does not exist, or if it exists but is empty.

So the correct way to do this would be to specify nothing:

```php
return $row->hotel->name;
```

Does the function require the return of a specific data type and is it strict about this? If we can't satisfy the specification, we should throw an exception.

```php
function getHotelName(int $hotelId): string
{
   // implementation

   if ($row->hotel->name === null) {
      throw new \Exception('Hotel name does not exist.');
   }

   return $row->hotel->name;
}
```

Character and data type encoding inconsistencies
--------------------------------------------

I not infrequently encounter constructions along the lines of:

```php
echo "{$returnCode}" . self::CRLF;
```

Which is better written as:

```php
echo $returnCode ."\n";
```

The use of quotes around the variable is unnecessary in this case and just makes it harder to read because it is extra unnecessary characters. At the same time, wrapping lines of type `CRLF` is not modern and it is better to use only `\n`, which is native to `UTF-8` encoding.

Properly, we could still validate the data type for the code. For example, that it is a number and possibly return a replacement. This can be done with the <a href="/ternary-operator">ternary operator</a>:

```php
echo (is_int($returnCode) ? $returnCode : '?') . "\n";
```

> Note: Using the `is_int()` function indicates poor application design, because it should never happen that we don't know the data type of a variable.

Wrapping the output string in apostrophes
---------------------------------------

It is often necessary to enclose the output string from a variable in quotes or apostrophes in the exception text. However, this unnecessarily creates "payoff" for both types of string delimiters, and if the string starts and ends with the same character, it is not possible to quickly determine unambiguously where the string started and ended in the case of multiple strings if an apostrophe appears inside:

```php
throw new \Exception(__METHOD__ . ": Unsupported data type '{$fileType}'");
```

I personally solve this by enclosing it in a different character type (square brackets work well for me), so that the beginning and end of the string can be seen elegantly:

```php
throw new \Exception(__METHOD__ . ': Unsupported data type ['' . $fileType . ']');
```

Or:

```php
throw new \Exception("Status '{$status}' is invalid");
```

Can be replaced by:

```php
throw new \Exception('Status [' . $status . '] is invalid');
```

Parsing line by line
--------------------

Quotation marks are useful, for example, for parsing by a new line:

```php
foreach(explode("\n", $text) as $line) {
	// implementation
}
```

They were specially created for this purpose.

However, if you need to process a more complex document (such as programming language source code or an HTML page), I recommend using **Tokenizer** together with <a href="/regex">regular expressions</a>.

Do not combine two methods of enclosing
-----------------------------------

If for some reason you're going to use quotes anyway, at least keep them consistent throughout.

This is a deterrent case:

```php
throw new \Exception("Image on URL does not exist. ResponseSize: " . strlen($result) . ')');
```

Where the beginning of the string is delimited by quotes and the end by apostrophes.

This defect occurs especially when using automatic formatting tools (such as **Code Sniffer**) that try to unify conventions, but do not always succeed.
