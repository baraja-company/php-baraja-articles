PHP funkcia stream_register_wrapper()
=====================================

> id: c05704b9-26dd-4918-acb7-520519c8b3e8
> slug:
> 	cs: funkce-stream-register-wrapper
> 	sk: php-funkcia-stream-register-wrapper
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '572d8dc2d5c185095a8409a1012f5dba'

Dostupnosť vo verziách: `PHP 4.3.0`

&Alias; <function>stream_wrapper_register</function>
<p>Registrácia obalu URL implementovaného ako trieda PHP


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$protocol` | `string` | *not* | Názov wrappera, ktorý sa má zaregistrovať. |
| `$classname` | `string` | *not* | Názov triedy, ktorá implementuje protokol. |
| `$flags` | `int` | *not* | Mala by byť nastavená na STREAM_IS_URL, ak je protokolom URL. Predvolená hodnota je 0, miestny prúd. |


Vrátené hodnoty
----------------

`bool`

Vráti `true` pri úspechu, inak `false` pri neúspechu.
</p>
<p>
stream_wrapper_register vráti false, ak
protokol už má spracovateľa.

Ďalšie zdroje
------------

[Oficiálna dokumentácia stream-register-wrapper](https://www.php.net/manual/en/function.stream-register-wrapper.php)
