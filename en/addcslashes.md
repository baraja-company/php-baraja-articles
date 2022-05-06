Addcslashes
===========

> id: '48278937-b1c5-479b-bac2-9b1ec552df4c'
> slug:
> 	cs: addcslashes
> 	en: addcslashes
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1f53fe14dd5fbf79958c645d15c4d204'

Support PHP4, PHP5

`addcslashes` - C-style slash string

Description
--------------------------

```php
string addcslashes (string $str, string $charlist)
```

Returns a string with backslashes before the characters that are specified in the charlist parameter.

Parameters
--------------------------

**str** Text string

**charlist**

characters to be removed. If charlist contains the characters `\n`, `\r`, and others, they are converted to C-style. Other non-alphanumeric ASCI characters with lengths less than 32 and greater than 126 change.

When you define a sequence of characters in a charlist argument, make sure you know what characters you put as the beginning and end of the range.

```php
echo addcslashes('foo[ ]', 'a..z');

// Values.
// Removes all upper and lower case letters
```

Return values
--------------------------

Returns the modified string.

Example
--------------------------

```php
$escaped = addcslashes($not_escaped, "\0..\37!@\177..\377");
```

charlist `\0..\37!@\177..\377`, removes all characters with ASCII code between 0 and 31.
