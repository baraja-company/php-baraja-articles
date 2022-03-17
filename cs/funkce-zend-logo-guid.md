PHP funkce zend_logo_guid()
===========================

> id: "180db505-820c-43dc-bc26-005d2226ea17"
> slug:
> 	cs: funkce-zend-logo-guid
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

> **Pozor:**
>
> Tato funkce je zastaralá a od PHP 5 již není dostupná.

Tato funkce vrací ID, které lze použít k zobrazení loga Zend pomocí vestavěného obrázku.

Například takto:

```php
echo '<img src="' . $_SERVER['PHP_SELF']
   . '?=' . zend_logo_guid() . '" alt="Zend Logo!">';
```

Alternativně lze použít funkci `php_logo_guid()`.

Parametry
--------------

Funkce nemá žádné vstupní parametry.

Návratové hodnoty
----------------

`string`

Vrátí hodnotu `PHPE9568F35-D428-11d2-A769-00AA001ACF42`.
