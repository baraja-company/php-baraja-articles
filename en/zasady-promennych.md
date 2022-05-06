Principles of writing variables
===============================

> id: '4c8e631b-b305-4169-8885-4f9506155999'
> slug:
> 	cs: zasady-promennych
> 	en: principles-of-writing-variables
> 
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '5528d9b73c1d1c07c330a58e4aeaa06a'

This is the second part of a tutorial series on PHP. In this episode, we will look at the basic rules for writing variables.

This page is just a quick overview. If you're looking for a detailed technical description of all the features, I've written a <a href="/variable">separate article</a>.

Basic syntax
--------------------------

Variables in PHP start with the dollar sign `$` followed immediately by the name.

```php
$animal = 'cat';
```

Strings (sequences of characters) are enclosed in quotes or apostrophes:

```php
$a = "quotes";
$b = 'apostrophes';
```

Digits are not enclosed in quotes:

```php
$a = 5;
$b = 10;
$c = 3.14159;
```

Variable names can only be characters of the English alphabet and numbers. The name always starts with a letter.

If the name consists of more than one word, it is customary to use the `camelCase` syntax (first letter lowercase and every other word starting with a capital letter):

```php
$cat = 'kitty';
$quickPocitac = 'Sure mine!';
$pocetRohuSingleborn = 1;
```


The name must not contain spaces, dashes, hooks, commas, quotation marks, parentheses, or other special characters. The only special character allowed is the `underscore`.

Decimal numbers shall be written with a period:

```php
$pi = 3.14159;
```


It can often be useful to perform mathematical operations directly when defining a variable:

```php
$a = 5;
$b = 3;
$c = $a + $b; // add 5 + 3
echo $c; // prints 8
```


Correct insertion of a quotation mark or apostrophe
--------------------------

Quotation marks and apostrophes must not be combined arbitrarily. For example, if we decide to use a quotation mark, we must also end the string with the quotation mark and not use it inside.

This is therefore wrong:

```php
echo "<img src="image.gif">";
```


Because it is not clear where the string starts and ends. Quotation marks and apostrophes are therefore not possible to enclose.

One possible solution is called <a href="/escapovani">escaping</a>, where the problematic character is preceded by a backslash.

```php
echo "<img src="image.gif">";
```


The backslash says that the next character will be exactly the one we want to use.

However, for outputting HTML code, it is preferable to enclose the entire string in apostrophes and then use quotes in the normal way:

```php
echo '<img src="image.gif">';
```


Alternatively, you can also rotate it:

```php
echo "<img src='image.gif'>";
```


Populating a variable from a url or form
--------------------------

Addresses containing a question mark carry information about the input variables, so for example `index.php?page=contacts` denotes the variable `page` with the value `contacts`. The value of this variable is read as `$_GET['page']`.

The question mark character is in no way related to the name of the file on disk. It is always the same file to which we pass parameters in the address.

I discuss this issue in detail in my article on <a href="/methods-odesilani-dat">methods of sending data</a>.

Defining the contents of a variable from an address
--------------------------

Some variables are available at the time the script is run (and can therefore be used straight away), these are called **superglobal variables**. For example, if we want to read a value from a URL, we use the `$_GET` variable.
The usage is as follows:

```php
$a = $_GET['a'];

echo $a;
```


This script prints to the source code what it has in the URL after the question mark.

> Warning, this sample is not safe! If a rogue visitor were to pass, for example, HTML code into the URL, it would be inserted into the page and executed. Therefore, we must always treat the output; the `htmlspecialchars()` function is used for this.

```php
$a = $_GET['a'];

echo htmlspecialchars($a);
```


> If we access the page without specifying the `?a=anything` parameter, the `$_GET['a']` variable will not exist and PHP will throw an error message. We need to treat this condition with a condition and do nothing if the variable does not exist (or alternatively, output alternative content). The existence of the variable can be verified with the `isset()` function.

```php
if (isset($_GET['a'])) {
    $a = $_GET['a'];

    echo htmlspecialchars($a);
} else {
    echo 'Variable "a" does not exist!
}
```


Example with counting
--------------------------

With the variables from the URL address we can do dog pieces, for example, add them up and write the result directly:

```php
echo $_GET['a'] + $_GET['b'];
```


If we want to include more input parameters in the URL, we need to separate them with an ampersand (`&`). So the address might look like this: `index.php?a=5&b=3`.

Linking text inputs (strings)
--------------------------

We can also easily link 2 text inputs (strings). This is done using the dot operator. You can link in a variable or when listing.

```php
$a = 'dog';
$b = 'cat';

echo $a . ' a ' . $b;
```


Print `dog and cat`.
