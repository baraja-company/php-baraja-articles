PHP funkce localeconv()
=======================

> id: ba98a788-6543-4763-ae8f-af22e937742c
> slug:
> 	cs: funkce-localeconv
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0.5`

Get numeric formatting information


Parametry
--------------

Funkce nemá žádné vstupní parametry.

Návratové hodnoty
----------------

`array`

localeconv returns data based upon the current locale
as set by setlocale. The associative array that is
returned contains the following fields:
<tr valign="top">
<td>Array element</td>
<td>Description</td>
</tr>
<tr valign="top">
<td>decimal_point</td>
<td>Decimal point character</td>
</tr>
<tr valign="top">
<td>thousands_sep</td>
<td>Thousands separator</td>
</tr>
<tr valign="top">
<td>grouping</td>
<td>Array containing numeric groupings</td>
</tr>
<tr valign="top">
<td>int_curr_symbol</td>
<td>International currency symbol (i.e. USD)</td>
</tr>
<tr valign="top">
<td>currency_symbol</td>
<td>Local currency symbol (i.e. $)</td>
</tr>
<tr valign="top">
<td>mon_decimal_point</td>
<td>Monetary decimal point character</td>
</tr>
<tr valign="top">
<td>mon_thousands_sep</td>
<td>Monetary thousands separator</td>
</tr>
<tr valign="top">
<td>mon_grouping</td>
<td>Array containing monetary groupings</td>
</tr>
<tr valign="top">
<td>positive_sign</td>
<td>Sign for positive values</td>
</tr>
<tr valign="top">
<td>negative_sign</td>
<td>Sign for negative values</td>
</tr>
<tr valign="top">
<td>int_frac_digits</td>
<td>International fractional digits</td>
</tr>
<tr valign="top">
<td>frac_digits</td>
<td>Local fractional digits</td>
</tr>
<tr valign="top">
<td>p_cs_precedes</td>
<td>
true if currency_symbol precedes a positive value, false
if it succeeds one
</td>
</tr>
<tr valign="top">
<td>p_sep_by_space</td>
<td>
true if a space separates currency_symbol from a positive
value, false otherwise
</td>
</tr>
<tr valign="top">
<td>n_cs_precedes</td>
<td>
true if currency_symbol precedes a negative value, false
if it succeeds one
</td>
</tr>
<tr valign="top">
<td>n_sep_by_space</td>
<td>
true if a space separates currency_symbol from a negative
value, false otherwise
</td>
</tr>
<td>p_sign_posn</td>
<td>
0 - Parentheses surround the quantity and currency_symbol
1 - The sign string precedes the quantity and currency_symbol
2 - The sign string succeeds the quantity and currency_symbol
3 - The sign string immediately precedes the currency_symbol
4 - The sign string immediately succeeds the currency_symbol
</td>
</tr>
<td>n_sign_posn</td>
<td>
0 - Parentheses surround the quantity and currency_symbol
1 - The sign string precedes the quantity and currency_symbol
2 - The sign string succeeds the quantity and currency_symbol
3 - The sign string immediately precedes the currency_symbol
4 - The sign string immediately succeeds the currency_symbol
</td>
</tr>
</p>
<p>
The p_sign_posn, and n_sign_posn contain a string
of formatting options. Each number representing one of the above listed conditions.
</p>
<p>
The grouping fields contain arrays that define the way numbers should be
grouped. For example, the monetary grouping field for the nl_NL locale (in
UTF-8 mode with the euro sign), would contain a 2 item array with the
values 3 and 3. The higher the index in the array, the farther left the
grouping is. If an array element is equal to CHAR_MAX,
no further grouping is done. If an array element is equal to 0, the previous
element should be used.

Další zdroje
------------

https://php.net/manual/en/function.localeconv.php
