Htmlspecialchars
================

> id: '46f04729-3956-4889-bb40-58362cb46b2a'
> slug:
> 	cs: htmlspecialchars
> 	en: htmlspecialchars
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0743dd02-12d0-4766-ba42-8fd7e9c4ae8a'
> sourceContentHash: '566e10c57527a45f32906beb6c21430f'

Htmlspecialchars() is a function to convert special characters to HTML entities.

Description
-----

```php
$variable = htmlspecialchars($text);
```

Some special characters have special meaning for browsers, so they should be converted to entities. This prevents general script safety and prevents the page from being rendered incorrectly.

It is most commonly used to protect forms and any place where the user inserts text and is at risk of inserting HTML tags.

| Character | Note | Changes to
|------|-------------------------|-----------
| `&` | ampersand | `&amp;`
| `"` | double quote (changes when `ENT_NOQUOTES` is disabled) | `&quot;`
| `'` | apostrophe (changes when `ENT_QUOTES` is enabled) | `&#039;`
| `<` | less than, HTML bracket | `&lt;`
| `>` | greater than, HTML bracket | `&gt;`

Parameter
--------

**String** to convert

**flags** Different behavior settings

**charset** Specifies the character set (encoding). The default character set is `ISO-8859-1`.

You can use `ISO-8859-1`, `ISO-8859-15`, `UTF-8`, `cp866`, `CP1251`, `CP1252`, and `KOI8-R`.

> Note: Support only from PHP 4.3.0 and later. Any other character sets are not recognized and supported.

**double_encode** When `double_encode` is disabled, PHP will not encode existing HTML entities, the default is to convert everything.

Return values
-----------------

Convert string.

If the string contains invalid units, within the given charset in `ENT_IGNORE` (not set), an empty string is returned.

Changes in versions
----------------

| Version | Note
|-------|---------
| 5.4.0 | Adding constants `ENT_SUBSTITUTE`, `ENT_DISALLOWED`, `ENT_HTML401`, `ENT_XML1`, `ENT_XHTML` and `ENT_HTML5`.
| 5.3.0 | Adding the `ENT_IGNORE` constant.
| 5.2.3 | Adding the `double_encode` parameter.
| 4.1.0 | Adding the `charset` parameter.

Example
-------

```php
$new = htmlspecialchars(
	'<a href="test">Test<a>',
	ENT_QUOTES
);

echo $new; // <a href="test">Test<a>
```
