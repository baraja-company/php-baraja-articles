Imprimer
========

> id: '23d672c5-b347-43b8-bd08-8ccb7ecca33f'
> slug:
> 	cs: print
> 	fr: imprimer
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7f7c3c32d9f3c1efd794ca38b7db384

`print` - Sortie de la chaîne de caractères

Description
--------------------------

```php
print 'Bonjour, le monde !';
```

`print()` n'est pas une vraie fonction (c'est une construction du langage), donc vous n'avez pas besoin d'utiliser les parenthèses.

Paramètres
--------------------------

- **arg**

Paramètre de sortie

Valeurs de retour
--------------------------

Renvoie toujours le chiffre 1.

Note
--------------------------

Remarque : Comme il s'agit d'une **construction linguistique** (et non d'une fonction), elle ne peut pas être chargée dans une variable.

Exemple
--------------------------

```php
print "Bonjour, le monde";

print "print" peut produire plusieurs lignes de texte, mais attention à la balise HTML.
parce que ça ne s'imprime pas. C'est ce que le <a href="/nl2br">Nl2br</a>.";

// Exemple de connexion avec une variable
$a = 'php';

print 'J'aime' . $a; // J'aime php
```

**print** est exactement la même fonction que **echo**. Si vous cherchez plus d'informations, consultez l'article <a href="/echo">echo</a>.
