PHP function htmlspecialchars - escaping characters
===================================================

> id: '2c8a5ae5-af3a-415f-bda0-5d291cadbea2'
> slug:
> 	cs: funkce-htmlspecialchars
> 	en: php-function-htmlspecialchars---escaping-characters
> 
> publicationDate: '2019-11-26 11:56:35'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '557212fbd327240294c922562d119caa'

Available in `PHP 4, 5, 7`.

Converts special characters that have meaning in HTML to HTML entities that can be rendered in the normal way.

Parameters
---------

| Parameter | Data type | Default value | Note |
|------------------|----------|--------------|-----|
| `$string` | `string` | *not* | [String](https://www.php.net/manual/en/language.types.string.php) to be converted. |
| `$flags` | `int` | `ENT_COMPAT` | A bitmask of one or more of the following flags, which specify how to handle quotes, invalid code unit sequences and the used document type. The default is `ENT_COMPAT | ENT_HTML401`. |
| `$encoding` | `string` | `'UTF-8'` | Defines the encoding used in conversion. If omitted, the default value for this argument is `ISO-8859-1` in versions of PHP prior to `5.4.0`, and UTF-8 from `PHP 5.4.0` onwards. |
| `$double_encode` | `bool` | `true` | When `double_encode` is turned off PHP will not encode existing html entities, the default is to convert everything. |

Return values
-----------------

`string` Modified string.

Other switches
---------------

| Constant | Description |
|------------------|-------|
| `ENT_SUBSTITUTE` | Replace invalid code unit sequences with a Unicode Replacement Character `U+FFFD (UTF-8)` or `&amp;#FFFD;` (otherwise) instead of returning an empty string.
| `ENT_DISALLOWED` | Replace invalid code points for the given document type with a Unicode Replacement Character `U+FFFD (UTF-8)` or `&amp;#FFFD`; (otherwise) instead of leaving them as is. This may be useful, for instance, to ensure the well-formedness of XML documents with embedded external content.
| `ENT_HTML401` | Handle code as HTML 4.01. |
| `ENT_XML1` | Handle code as XML 1. |
| | `ENT_XHTML` | Handle code as XHTML. |
| | `ENT_HTML5` | Handle code as HTML 5. |

Other resources
------------

[Official htmlspecialchars documentation](- [Official php.net documentation](https://www.php.net/manual/en/function.htmlspecialchars.php))
- [STRING data type information](https://www.php.net/manual/en/language.types.string.php)
- [UNICODE security and encoding information](https://unicode.org/reports/tr36/#Deletion_of_Noncharacters)
