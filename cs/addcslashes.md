Addcslashes
===========

> id: "48278937-b1c5-479b-bac2-9b1ec552df4c"
> slug:
> 	cs: addcslashes
> 
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "3666a8a6-f2a3-405d-8263-bd53c4301fb3"

Podpora: PHP4, PHP5

`addcslashes` - Řetězec lomítek ve stylu C

Popis
--------------------------

```php
string addcslashes (string $str, string $charlist)
```

Vrací řetězec se zpětnými lomítky před znaky, které jsou uvedeny v charlist parametr.

Parametry
--------------------------

**str** Textový řetězec

**charlist**

znaky, které se mají odstranit. Pokud charlist obsahuje znaky `\n`, `\r`, a další,  jsou přeměněny v C-stylu. Ostatní non-alfanumerické ASCI znaky s délkou nižší než 32 a vyšší než 126 změn.

Když definujete sekvence znaků v charlist argument, ujistěte se, že víte, jaké znaky jste dali jako začátek a konec rozsahu.

```php
echo addcslashes('foo[ ]', 'A..z');

// Hodnoty:  \f\o\o\[ \]
// Odstraní všechny malá a velká písmena
```

Návratové hodnoty
--------------------------

Vrátí upravený řetězec.

Příklad
--------------------------

```php
$escaped = addcslashes($not_escaped, "\0..\37!@\177..\377");
```

charlist `\0..\37!@\177..\377`, odstraní všechny znaky s ASCII kódem v rozmezí 0 až 31.
