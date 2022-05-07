Fonctions pures en PHP
======================

> id: d94c18a9-8bc4-4377-8042-e7c8b48320a2
> slug:
> 	cs: pure-funkce
> 	fr: fonctions-pures-en-php
> 
> publicationDate: '2021-10-27 10:30:00'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: e47a3507ddb189a218ad8e3a38703e5c

En programmation fonctionnelle, il existe un concept de **fonction pure**, qui fait référence à une fonction qui renvoie toujours la même sortie pour la même entrée (c'est-à-dire qu'elle est déterministe) et qui, en même temps, ne souffre d'aucun effet secondaire (c'est-à-dire qu'elle n'affecte pas son environnement).

Ce à quoi ressemble une fonction pure
----------------------

Exemple d'une fonction pure :

```php
// Il s'agit d'une fonction pure
function add(int $a, int $b): int
{
	return $a + $b;
}
```

Il s'agit d'une fonction pure car la sortie est toujours la même en fonction des arguments d'entrée.

Ce qui n'est pas une fonction pure
-------------------

```php
// C'est une fonction impure
function add(int $a, int $b): int
{
	echo 'Ajoutant...';
	file_put_contents('file.txt', 'Valeur :' . $a);
	return $a + $b;
}
```

Ce type de fonction n'est pas pur car la fonction modifie le système de fichiers. Un autre type de fonction impure est lorsqu'elle interagit avec la base de données, imprime à l'écran, etc.
