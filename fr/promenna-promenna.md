Variables Variables
===================

> id: a0baeb3a-ab6e-4dd9-b1bc-b1872a12ee08
> slug:
> 	cs: promenna-promenna
> 	fr: variables-variables
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '590515cc711ec27cc0f0bfe190c8fbff'

**Avertissement : ** Cet article a été écrit il y a plusieurs années et certaines informations peuvent être dépassées ou incorrectes. Veuillez garder cela à l'esprit lors de votre lecture.

**Les variables** ne sont pas destinées à un déploiement commun (elles résolvent des problèmes qui peuvent être résolus par d'autres moyens), elles sont principalement utilisées pour rendre les écritures plus concises et les accès à la mémoire plus complexes.

Prenons l'exemple suivant :

```php
$x = 25;                  // contient 25
$nacitana_promenna = 'x'; // contient "x"
$y = $$nacitana_promenna; // contient 25
echo $y;                  // imprime 25
```

Remarquez les deux dollars qui se suivent. Dans ce cas, la variable $y sera chargée avec la valeur de la variable qui a le nom contenu dans la variable $nacitana_variable.

Un peu déroutant, hein ? C'est pourquoi il vaut mieux ne pas utiliser de variables.
> **Note:** Les variables sont une spécialité de PHP à cause du signe dollar. Dans d'autres langues, le début du nom de la variable n'est marqué d'aucun caractère. Vous ne pouvez donc pas utiliser de variables car il serait ambigu de savoir quand il s'agit d'une variable classique et quand il ne l'est pas.
