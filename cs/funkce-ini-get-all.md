PHP funkce ini_get_all()
========================

> id: "4276139a-34ff-4e6b-b504-8a44cbbe4e73"
> slug:
> 	cs: funkce-ini-get-all
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.2.0`

Gets all configuration options


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$extension` | `string` | null, | An optional extension name. If set, the function return only options specific for that extension. |
| `$details` | `bool` | null | Retrieve details settings or only the current value for each setting. Default is true (retrieve details). |


Návratové hodnoty
----------------

`array`

an associative array with directive name as the array key.
</p>
<p>
When details is true (default) the array will
contain global_value (set in
&php.ini;), local_value (perhaps set with
ini_set or &htaccess;), and
access (the access level).
</p>
<p>
When details is false the value will be the
current value of the option.
</p>
<p>
See the manual section
for information on what access levels mean.
</p>
<p>
It's possible for a directive to have multiple access levels, which is
why access shows the appropriate bitmask values.

Další zdroje
------------

[Oficiální dokumentace funkce ini-get-all](https://www.php.net/manual/en/function.ini-get-all.php)
