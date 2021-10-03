PHP funkce get_meta_tags()
==========================

> id: d1dbc4f7-44ef-458e-b9d3-d88690b9b733
> slug:
> 	cs: funkce-get-meta-tags
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Extracts all meta tag content attributes from a file and returns an array


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` |  | The path to the HTML file, as a string. This can be a local file or an URL. |
| `$use_include_path` | `bool` | null | Setting use_include_path to true will result in PHP trying to open the file along the standard include path as per the include_path directive. This is used for local files, not URLs. |


Návratové hodnoty
----------------

`array`

an array with all the parsed meta tags.
</p>
<p>
The value of the name property becomes the key, the value of the content
property becomes the value of the returned array, so you can easily use
standard array functions to traverse it or access single values.
Special characters in the value of the name property are substituted with
'_', the rest is converted to lower case. If two meta tags have the same
name, only the last one is returned.

Další zdroje
------------

https://www.php.net/manual/en/function.get-meta-tags.php
