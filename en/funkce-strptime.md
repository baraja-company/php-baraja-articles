PHP function strptime()
=======================

> id: ed1276e2-0c36-48fb-ad8f-f6a55302cc6b
> slug:
> 	cs: funkce-strptime
> 	en: php-function-strptime
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '2cf5a0e197af7e021ff24fbc2ae45c20'

Availability in versions: `PHP 5.1.0`

Parse a time/date generated with <function>strftime</function>


Parameters
--------------

| Parameter | Data type | Default value | Note |
|-----|-----|-----|-----|
| `$date` | `string` | *not* | The string to parse (e.g. returned from strftime) |
| `$format` | `string` | *not* | The format used in date (e.g. the same as used in strftime). |


Return values
----------------

`array`

|bool an array or false on failure.
</p>
<p>
<table>
The following parameters are returned in the array
<tr valign="top">
<td>parameters</td>
<td>Description</td>
</tr>
<tr valign="top">
<td>"tm_sec"</td>
<td>Seconds after the minute (0-61)</td>
</tr>
<tr valign="top">
<td>"tm_min"</td>
<td>Minutes after the hour (0-59)</td>
</tr>
<tr valign="top">
<td>"tm_hour"</td>
<td>Hour since midnight (0-23)</td>
</tr>
<tr valign="top">
<td>"tm_mday"</td>
<td>Day of the month (1-31)</td>
</tr>
<tr valign="top">
<td>"tm_mon"</td>
<td>Months since January (0-11)</td>
</tr>
<tr valign="top">
<td>"tm_year"</td>
<td>Years since 1900</td>
</tr>
<tr valign="top">
<td>"tm_wday"</td>
<td>Days since Sunday (0-6)</td>
</tr>
<tr valign="top">
<td>"tm_yday"</td>
<td>Days since January 1 (0-365)</td>
</tr>
<tr valign="top">
<td>"unparsed"</td>
<td>the date part which was not
recognized using the specified format</td>
</tr>
</table>

Other resources
------------

[Official strptime documentation](https://www.php.net/manual/en/function.strptime.php)
