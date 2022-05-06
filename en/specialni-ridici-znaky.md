Special control characters in PHP
=================================

> id: '888e6ee4-2ff3-4f0d-8f5a-4741e62e11d0'
> slug:
> 	cs: specialni-ridici-znaky
> 	en: special-control-characters-in-php
> 
> publicationDate: '2021-11-24 14:05:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '2711968227a253040a0a6ed1464aa447'

PHP strings may contain special control characters that have different meanings in a particular context and do not necessarily behave like regular characters.

Many of these will already be intuitively familiar to you. Some are reserved for special uses and others are reserved for keyboard characters, for example.

Writing special characters
-----------------------

Special characters are written in double quotes.

So it is very simple:

```php
$message = "Hello\nworld.";
```

The preceding code contains a line break between `Hello` and `world`.

Special character table
-------------------------

If the string is enclosed in double quotes ("), PHP will interpret the following escape sequences as special characters:

| Sequence | Meaning |
|----------|--------|
| `\n` | linefeed (`LF` or `0x0A (10)` in ASCII) |
| `\r` | carriage return (`CR` or `0x0D (13)` in ASCII) |
| ``t` | horizontal tab (`HT` or `0x09 (9)` in ASCII) |
| `\v` | vertical tab (`VT` or `0x0B (11)` in ASCII) |
| `\e` | escape (`ESC` or `0x1B (27)` in ASCII) |
| `\f` | form feed (`FF` or `0x0C (12)` in ASCII) |
| `\\\` | backslash |
| `\$` | dollar sign |
| `\"` | double-quote |
| `[0-7]{1,3}` | The sequence of characters corresponding to a regular expression is a character in octal notation that silently overflows into a byte. (e.g. `"\400" === "\000"`) |
| `\x[0-9A-Fa-f]{1,2}` | The sequence of characters corresponding to a regular expression is a character in hexadecimal notation. |
| `\u{[0-9A-Fa-f]+}` | the sequence of characters matching the regular expression is a Unicode code point, which will be output to the string as a UTF-8 representation of that code point.

As with single-quoted strings, a backslash will be output when escaping any other character.

When delimiting strings with quotes, keep in mind that the contained variables will be expanded (the values of the variables will be written directly to the string). This behavior can be extremely dangerous.
