PHP function nl2br()
====================

> id: cbbf888a-9af2-49b2-984e-2d836b1337be
> slug:
> 	cs: funkce-nl2br
> 	en: php-function-nl2br
> 
> publicationDate: '2020-02-16 18:48:19'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '68e1dd535c4e114e31699eb3faf7ebfe'

This function converts line breaks (`\n`) in a string to the HTML tag `<br>`.

Parameters
---------

| Parameter | Data type | Default value | Note |
|-------------|------------|--------|-----|
| `$string` | `string` | *not* | Input string. |
| `$is_xhtml` | `bool` | `null` | Switches the escaping method according to the context. |

Detailed description
--------------

```php
$retezec = 'text
next text
and something else';

echo nl2br($retezec);
```

**Returns:**

```html
text<br>
more text<br>
and something else
```

Converts broken lines in text to html tags. This tag is used in places where the user enters any text (textarea) and there is a risk of using multiple lines of text.

The treatment of classic inputs (`type="text"`) is meaningless, as multi-line text cannot be entered here.

**Note:** As of PHP 4.0.5, the `nl2br()` XHTML function is eligible. All versions before 4.0.5 will return a string with a tag inserted before the line breaks instead of `<br />`.

Example
-------

```php
echo nl2br("Welcome\r\nThis is my HTML document", false);
```

Returns:

```html
Welcome<br>
This is my HTML document
```

Return values
----------------

`string`

Returns an edited string including HTML tags.

Changes in versions
----------------

| Version | Note
|-------|---------
| 5.3.0 | Added optional is_xhtml parameter.
| 4.0.5 | `nl2br ()` is now XHTML compatible. All earlier versions will return a string with line breaks inserted instead of `<br />`.

Other resources
------------

[Official nl2br function documentation]([Official nl2br() documentation](https://www.php.net/manual/en/function.nl2br.php))
