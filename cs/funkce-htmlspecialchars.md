PHP funkce htmlspecialchars - escapování znaků
==============================================

> id: "2c8a5ae5-af3a-415f-bda0-5d291cadbea2"
> slugCS: funkce-htmlspecialchars
> publicationDate: "2019-11-26 11:56:35"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4, 5, 7`.

Převede speciální znaky, které mají v HTML význam na HTML entity, které lze běžným způsobem vykreslit.

Parametry
---------

| Parametr         | Datový typ | Výchozí hodnota | Poznámka |
|------------------|----------|--------------|-----|
| `$string`        | `string` |              | [Řetězec](https://www.php.net/manual/en/language.types.string.php) který bude převeden. |
| `$flags`         | `int`    | `ENT_COMPAT` | A bitmask of one or more of the following flags, which specify how to handle quotes, invalid code unit sequences and the used document type. The default is `ENT_COMPAT | ENT_HTML401`. |
| `$encoding`      | `string` | `'UTF-8'`    | Defines encoding used in conversion. If omitted, the default value for this argument is `ISO-8859-1` in versions of PHP prior to `5.4.0`, and UTF-8 from `PHP 5.4.0` onwards. |
| `$double_encode` | `bool`   | `true`       | When `double_encode` is turned off PHP will not encode existing html entities, the default is to convert everything. |

Návratové hodnoty
-----------------

`string` Upravený řetězec.

Další přepínače
---------------

| Konstanta        | Popis |
|------------------|-------|
| `ENT_SUBSTITUTE` | Replace invalid code unit sequences with a Unicode Replacement Character `U+FFFD (UTF-8)` or `&amp;#FFFD;` (otherwise) instead of returning an empty string.
| `ENT_DISALLOWED` | Replace invalid code points for the given document type with a Unicode Replacement Character `U+FFFD (UTF-8)` or `&amp;#FFFD`; (otherwise) instead of leaving them as is. This may be useful, for instance, to ensure the well-formedness of XML documents with embedded external content. |
| `ENT_HTML401`    | Handle code as HTML 4.01. |
| `ENT_XML1`       | Handle code as XML 1. |
| `ENT_XHTML`      | Handle code as XHTML. |
| `ENT_HTML5`      | Handle code as HTML 5. |

Další zdroje
------------

- [Oficiální dokumentace funkce na php.net](https://php.net/manual/en/function.htmlspecialchars.php)
- [Informace o datovém typu STRING](https://www.php.net/manual/en/language.types.string.php)
- [Informace o bezpečnosti a kódování UNICODE](https://unicode.org/reports/tr36/#Deletion_of_Noncharacters)
