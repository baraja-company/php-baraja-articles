How to write your first PHP script
==================================

> id: '2d6986dc-f24b-4ae5-b24c-d5bb9bf94512'
> slug:
> 	cs: prvni-script
> 	en: how-to-write-your-first-php-script
> 
> perex: Jak napsat první PHP script? Úvod do PHP pro začátečníky.
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: a5fab0e5edf99323140e497a6fe52322

Before we write our first PHP script, we first need to theoretically explain how to load a page using PHP.

1. First, the user calls a specific URL in their web browser, for example `https://baraja.cz`
2. Then the web browser builds a `request`, which is a special request to the web server that is sent to the Internet. It contains information about the requested page, basic browser identification and settings, information about cookies, and so on.
3. This `request` travels across the Internet to a web server (most often `Apache`), which reads the request and starts compiling a response.
4. Since the called URL is a PHP script and the request is for a file named `index.php`, `Apache` reads the `index.php` file from the root directory on disk and passes it to the `PHP interpreter`, which is a program that can process the PHP code itself and build `HTML code` based on it, which is sent back to the user.
5. Once the HTML code is compiled, the response is sent back to the user (this is called a `response`) and the web browser renders the page in the normal way, as if it were pure HTML.

> Note that the web browser does not learn anything about the contents of the PHP script, but only processes the generated HTML, so your scripts and server content remain safe.

Let's create the first script
----------------------------

> Writing your first script assumes that you have a web server running on your computer. For Windows, <a href="https://www.apachefriends.org/index.html">XAMPP</a> is best (download PHP version 7.0 or later), and <a href="https://www.apachefriends.org/index.html">XAMPP</a> works exactly the same on Mac as it does on Windows. For Linux, I recommend <a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">LAMP server</a> (this site runs on Lamp server as well).

The name of the PHP script file must end with the `.php` extension so that the web server knows that we want to process it according to PHP rules. So let's create an example `index.php` file that will contain the code for the main page of our website.

Opening the file
--------------------------

Open this file in a suitable text editor for writing source code.

In Windows, for example, <a href="https://www.sublimetext.com">Sublime Text</a> is a good place to start, as it nicely colors the syntax (language rules) and makes the code easier to read. Later on, I recommend buying <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, which is used a lot in companies and offers the possibility to program in multiple people.

Write the basic structure of an HTML page
--------------------------

You probably already know the basic structure of an HTML page:

```html
<!DOCTYPE HTML>
<html>
    <head>
    <title>My first PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>

    </body>
</html>
```


All HTML code will be processed in the normal way and will be a great help for designing the site. PHP uses the principles of HTML and CSS to a great extent.

Separation of PHP script from HTML code
--------------------------

PHP is mainly a templating language that generates custom content at appropriate places in the code. To be able to clearly tell what is HTML and what is PHP, we need to use a separator tag.

Currently, it is best to use the notation with `<?php` and `?>`.

```php
<?php
    // here will be the PHP code
?>
```

> We use the `?>` terminator if we want to use some other HTML code. If there is no more HTML code at the end of the PHP script, it is preferable not to include the `?>` tag, so that there are no unnecessary white space and line breaks at the end of the page, which can be inserted by the text editor.

In the past, the `<?` tag has been used a lot instead of `<?php`, but it may not always be supported.

Wrapper tags can be placed anywhere in the HTML code, such as in the body of the page:

```html
<!DOCTYPE HTML>
<html>
    <head>
    <title>My first PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>
    <?php
        // here will be the PHP code
    ?>
    </body>
</html>
```


Basic constructs
--------------------------

Among the very basic building blocks are:

- <a href="/echo">Listing content in the source code</a>
- <a href="/variable">Variables</a>
- <a href="/if">Conditions</a>

In this section, we will demonstrate a simple listing of content to source code using a variable.

The principle of source code construction
--------------------------------

All constructs (language expressions), statements, and functions are separated by semicolons to make it unambiguous where the current construct is valid from and to.

A semicolon is usually followed by a line break.

Symbolically written:

```php
command;
next command;
variable x = its value;

list the variable x;

save to file;
```


Extract to source code
--------------------------

The <a href="/echo">echo</a> construct is used to list the content.

It is very easy to use:

```php
echo 'Hello world!';
```


It then prints the text "Hello World!" into the HTML code. Try the sample.

> All other demos will only contain the inside of the PHP code. The surrounding HTML code is free for you to make up your own mind (for example, use the sample at the beginning of this article).

Variables
--------------------------

Variables are virutal memory locations that store data and are used to move it around. The name of a variable always starts with a `dollar`, followed by the `name` itself, and then its `value`.

> I have summarized a detailed description of how variables work in <a href="/variable">a separate article on variables</a>.

```php
$oblibeneNumber = 1024;
$nameAuthor = 'Jan Barasek';

echo $oblibeneNumber;
echo '<br>';
echo $nameAuthor;
```


> The variable name should express what the variable actually contains, to make the code clearer. Also note the insertion of the HTML tag `<br>` to indent the text. You should already be familiar with this tag from HTML.

What is output in the `echo` construct is called a string (a sequence of characters). Individual strings can be concatenated with a period (`.`) to reduce the output to a single line:

```php
$oblibeneNumber = 1024;
$nameAuthor = 'Jan Barášek';

echo $oblibeneCislo . '<br>' . $nameAuthor;
```


> After concatenating the strings with a period, the whole thing will be seen as one big string.

Variable operations
--------------------------

Between variables, all basic <a href="/mathematics">mathematical operations</a> work perfectly intuitively as expected.

Let's define 2 variables and put numbers into them:

```php
$x = 5; // defines a variable x with a value of 5
$y = 3; // defines the variable y with value 3

echo $x + $y; // adds the variables and prints 8
```


> Note that the equals sign (`=`) is not used to perform a mathematical operation, so you cannot write equations, for example. PHP just acts like a calculator in this respect.

If we don't want to use variables, we can perform the operations directly. So the location of the operations doesn't matter and they will evaluate anywhere.

```php
echo 5 + 3; // prints 8
```


Alternatively, we can add the variables and store the result in another variable:

```php
$x = 5;
$y = 3;
$z = $x + $y; // variable $z contains 8

echo $z; // prints 8
```

In the next part, we'll learn the complete basics of <a href="/sets-of-variables">variable definitions and usage</a>.
