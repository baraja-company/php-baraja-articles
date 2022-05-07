Addcslashes
===========

> id: '48278937-b1c5-479b-bac2-9b1ec552df4c'
> slug:
> 	cs: addcslashes
> 	fr: addcslashes
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1f53fe14dd5fbf79958c645d15c4d204'

Support de PHP4, PHP5

`addcslashes` - chaîne de caractères obliques de style C

Description
--------------------------

```php
string addcslashes (string $str, string $charlist)
```

Renvoie une chaîne de caractères avec des barres obliques inversées avant les caractères spécifiés dans le paramètre charlist.

Paramètres
--------------------------

**str** Chaîne de texte

**charlist**

les caractères à supprimer. Si charlist contient les caractères `\n`, `\r`, et autres, ils sont convertis en style C. Les autres caractères ASCI non alphanumériques dont la longueur est inférieure à 32 et supérieure à 126 changent.

Lorsque vous définissez une séquence de caractères dans un argument charlist, assurez-vous de savoir quels caractères vous mettez comme début et fin de la plage.

```php
echo addcslashes('foo[ ]', 'A..z');

// Valeurs : \f\o\o\o[ \]
// Supprime toutes les lettres minuscules et majuscules
```

Valeurs de retour
--------------------------

Renvoie la chaîne de caractères modifiée.

Exemple
--------------------------

```php
$escaped = addcslashes($not_escaped, "\0..\37!@\177..\377");
```

charlist `\0..\37!@\177..\377`, supprime tous les caractères dont le code ASCII est compris entre 0 et 31.
