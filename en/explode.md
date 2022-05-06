PHP function Explode - split string by separator
================================================

> id: '4dc7cec6-0e96-4a6b-aee8-32c8817ba11e'
> slug:
> 	cs: explode
> 	en: php-function-explode---split-string-by-separator
> 
> perex: Rozdělení řetězce na více částí podle oddělovače.
> publicationDate: '2019-11-26 11:39:36'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '66725772be4aae0df8af399e7ce2ba07'

Explode is used to easily split a string by a separator.

- It returns the individual results as an array numbered from zero,
- you can't insert an array, only the string is input,
- cannot change the delimiter during parsing, cannot select multiple delimiters.

| Support | PHP 4 and later |
|---------------|-----------------|
| Brief description | Splitting a string into an array by separator.
| Requirements | None
| Note | Cannot insert an array, only a string.

Often we need to split a string according to some simple rule. For example, a comma separated list of numbers.

The explode() function is great for this, taking the separator (separating the string) as the first parameter and the data itself as the second parameter:

```php
$cisla = '3, 1, 4, 1, 5, 9';
$parser = explode(', ', $cisla);

foreach ($parser as $cislo) {
	echo $cislo . '<br>';
	// Here we can further process the numbers
}
```

But what if the numbers are separated by commas, but there are spaces around them?

The solution is to parse by the smallest common substring and then remove unwanted characters around it (spaces and other whitespace):

```php
$cisla = '3, 1,4, 1 , 5 , 9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
     echo trim($cislo) . '<br>';
     // Here we can further process the numbers
}
```

In this case, the `trim()` function neatly removes the whitespace around the characters (spaces, tabs, line breaks, ...), giving us only clean data.

Limit, limiting the number of parsed entities into an array
--------------------------

> TIP: For many examples, explode() is not suitable and it is much better to use regular expressions.

However, we often only want to parse the data up to a certain distance, and the third (optional) parameter limit can be used for this purpose.

For example, let's have colon-separated structured data where we want to get the content after the first colon and ignore the other colons.
Example:

```php
$cas = 'format: "j.n.Y - H:i"';
```

If we were to parse the string as just:

```php
$parser = explode(':', $cas);
```

We would get these 3 substrings (in other cases there could be many more):

```php
'format'
' 'j.n.Y - H'
'i"'
```

Therefore, we set a limit to how many elements to get in the parser (and possibly all the others will be appended to the end of the last element):

```php
$parser = explode(':', $cas, 2);

// returns:
echo $parser[0]; // format
echo $parser[1]; // "j.n.Y - H:i"
```

> **Note:** Unwanted quotes can be removed from the string, for example, by using the `trim()` function:

```php
echo trim($parser[1], '"'); // the second parameter specifies the map of characters to remove
```

Between, getting the string between two strings
--------------------------

Often we need to get a string that is bounded by two other strings. There is no function for this directly in PHP, but we can write it ourselves:

```php
function between(string $left, string $right, string $data): string
{
   $l = explode($left, $data);
   $r = explode($right, $l[1]);

   return $r[0];
}
```

Regular expressions
--------------------------

Much more elegant splitting and working with strings can be achieved using <a href="/regex">regular expressions</a>, which I discuss on a separate page.

Similar functions
--------------------------

- <a href="/function-implode">Implode()</a> - concatenate an array into a string

IP address parsing example
--------------------------

```php
$ip = '10.0.0.138';
$parser = explode('.', $ip);
echo $parser[0]; // prints "10"
echo $parser[1]; // prints "0";
```

The variable `$ip` contains the input string, which is parsed according to the `.` delimiter; the return is an array. Parsing continues until the end of the string, unless a limit is specified.

Parameters
--------------------------

| # | Type | Description
|---|--------|------|
| 1 | string | Separation string.
| 2 | string | Parsed string.
| 3 | int | parsing limit. This is an optional parameter. Example:

```php
$text = 'PHP is my favorite language!';
$parser = explode(' ', $text, 1);

echo $parser[0]; // prints the first word
echo $parser[1]; // prints the rest of the text
echo $parser[2]; // prints nothing because a limit has been set!
```

Return values
--------------------------

The return is an array with a parsed string.

The indices are numbered from zero to `X` unless a limit is specified.

Differences from earlier versions
--------------------------

| PHP version | Description |
|-----------|-------|
| 5.1.0 | Added support for negative limit on passes.
| 4.0.1 | Added optional **limit** parameter.

Tips and notes
--------------------------

When using a negative **limit**, the number of elements from the end of the string is given.

Example:

```php
$str = 'one|two|three|four';

// positive limit
print_r(explode('|', $str, 2));

// negative limit (since PHP 5.1)
print_r(explode('|', $str, -1));
```

The following fields will be returned:

```php
Array
(
    [0] => one
    [1] => two|three|four
)

Array
(
    [0] => one
    [1] => two
    [2] => three
)
```
