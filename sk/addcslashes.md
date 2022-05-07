Addcslashes
===========

> id: '48278937-b1c5-479b-bac2-9b1ec552df4c'
> slug:
> 	cs: addcslashes
> 	sk: addcslashes
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1f53fe14dd5fbf79958c645d15c4d204'

Podpora PHP4, PHP5

`addcslashes` - reťazec lomítok v štýle C

Popis
--------------------------

```php
string addcslashes (string $str, string $charlist)
```

Vráti reťazec so spätnými lomkami pred znakmi, ktoré sú zadané v parametri charlist.

Parametre
--------------------------

**str** Textový reťazec

**zoznam znakov**

znaky, ktoré sa majú odstrániť. Ak charlist obsahuje znaky `\n`, `\r` a iné, sú prevedené do štýlu C. Ostatné nealfanumerické ASCI znaky s dĺžkou menšou ako 32 a väčšou ako 126 sa menia.

Keď definujete postupnosť znakov v argumente charlist, uistite sa, že viete, aké znaky ste vložili ako začiatok a koniec rozsahu.

```php
echo addcslashes('foo[ ]', 'A..z');

// Hodnoty: \f\o\o\o[ \]
// Odstráni všetky malé a veľké písmená
```

Vrátené hodnoty
--------------------------

Vracia upravený reťazec.

Príklad
--------------------------

```php
$escaped = addcslashes($not_escaped, "\0..\37!@\177..\377");
```

charlist `\0..\37!@\177..\377`, odstráni všetky znaky s ASCII kódom medzi 0 a 31.
