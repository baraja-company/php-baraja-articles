PHP funkce setlocale()
======================

> id: "7b94a46f-a7d9-4e65-80e5-b1b99db5e148"
> slugCS: funkce-setlocale
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Set locale information


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$category` | `int` |  | <p> <em>category</em> is a named constant specifying the category of the functions affected by the locale setting: |
| `$locale` | `string` |  | If locale is &null; or the empty string "", the locale names will be set from the values of environment variables with the same names as the above categories, or from "LANG". |
| `$_` | `string` | null |  |


Návratové hodnoty
----------------

`string`

the new current locale, or false if the locale functionality is
not implemented on your platform, the specified locale does not exist or
the category name is invalid.
</p>
<p>
An invalid category name also causes a warning message. Category/locale
names can be found in RFC 1766
and ISO 639.
Different systems have different naming schemes for locales.
</p>
<p>
The return value of setlocale depends
on the system that PHP is running. It returns exactly
what the system setlocale function returns.

Další zdroje
------------

https://php.net/manual/en/function.setlocale.php
