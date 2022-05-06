Construct include
=================

> id: '881cc6ef-b1dc-4a71-ab22-d1943ce8095b'
> slug:
> 	cs: include-function
> 	en: construct-include
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '53294e511809a77c08883100fd7416c6'

| Support | PHP 4, PHP 5, PHP 7
|---------------|---------
| Short Description | Appends another text file or script to the script.
| Requirements | Other text file or script to be inserted.
| Note | Cannot load external files.

Description
--------------------------

Inserts another text file or script into the page. Supports plain/text files.

Embedded files behave as if they were directly in the page.

Embedded scripts are automatically executed.

The embedded script transfers the value of variables.

> Cannot be loaded from external storage. Can only read plain unformatted text and PHP files.

Allowed path formats:

- `script.php` - file in the same directory, will be executed
- `script.html` - file in the same directory, will not be executed
- `./file.php` - file in the same directory, will be executed
- `../page.html`
- `folder\file\file.php` - write in Windows
- `address/DalsiAddress/file.php` - write on Unix systems

Slashes of type `****` and `**/**` can interconvert, so you don't have to deal with that.

Illegal path entry: `https://domena.pripona/slozka/soubor.php`

Similar functions
--------------------------

- <a href="/file-get-contents">file_get_contents()</a>

Example
--------------------------

```php
include 'file.php';
```

Inserts the `file.php` script into the page and executes it.

`vars.php`
```php
$color = 'green';
$fruit = 'apple';
```

`test.php`
```php
$color = '';
$fruit = '';

echo 'A ' . $color . ' ' . $fruit; // A

include 'vars.php';

echo 'A ' . $color . ' ' . $fruit; // A green apple
```

> WARNING: The following notation is not possible, transfer the contents of variables by defining them!

```php
include 'file.php?parameter=neco';
```

Return values
--------------------------

None, just inserts the file.

**NOTE:** Allows you to compare file contents, but this is a security risk. The order of the brackets matters! Example:

```php
if ((include 'file.php') == 'OK') {
    echo 'The value is "OK"';
}
```


> This is a potential security risk!
>
> Can be solved with **file_get_contents()**, **readfile()** or **fopen()**. Fopen() should only be used on .txt and .html files.
>
> A safer way to read a file is to define a custom function.

Example:

```php
$string = get_include_contents('somefile.php');

function get_include_contents(string $filename): ?string
{
    if (is_file($filename)) {
        ob_start();
        include (string) $filename;
        return ob_get_clean();
    }

    return null;
}
```

Notes and tips from other developers
--------------------------

**Jakub Vrána** wrote to me in the mail:

```php
include 'articles/' . $_GET['clanek'] . '.html';
```

This is extremely dangerous.

An attacker can pass a link to another directory using `../` or something similar as the article name, and sometimes it is possible to get rid of the ending by passing a null byte at the end.

You should at least use the `basename()` function, but better to allow only whitelist values.
