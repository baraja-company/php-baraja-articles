Mots-clés PHP
=============

> id: d58620d7-57a0-410a-ba73-31a1f9d984fb
> slug:
> 	cs: keywordy-jazyka-php
> 	fr: mots-cles-php
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '38fbdb53b4455903f3300c3c8b4a8ad2'

Presque tous les langages de programmation sont composés de "mots-clés", qui sont des expressions spéciales du langage ayant une signification particulière.

Un exemple de mot-clé est le mot `string`, `if` ou `echo`.

Il est important de noter que les mots-clés (parfois aussi `commandes`) <a href="/commandes-et-fonctions">ne sont pas des fonctions</a>, donc `echo`, par exemple, n'est pas non plus une fonction.

Il est bon de connaître la liste des mots-clés car ils ont une signification particulière en PHP et ne peuvent pas toujours être utilisés pour tout. Par exemple, le nom d'une classe ne doit pas être le même que l'un des mots-clés existants.

Exemple d'erreur d'analyse
-------------------

Lorsque vous essayez de traiter une classe nommée `String`, une `Erreur d'analyse` se produit car PHP ne sait pas si c'est un nom de classe ou un type de données :

C'est faux :

```php
class String
{
   // ...
}
```

Liste de mots-clés
-------------------

Mise à jour de la liste des mots-clés pour PHP 7.1 :

et ", " ou ", " xor ", " pour ", " faire ", " pendant ", " avant ", " comme ", " retour ", " mort ", " sortie ", " si ", " alors ", " sinon ", " si ", " nouveau ", " supprimer ", Essayez, lancez, attrapez, finissez, classe, fonction, chaîne de caractères, tableau, objet, ressource, variable,ool, booléen et nombre, `integer`, `float`, `double`, `real`, `string`, `array`, `global`, `const`, `static`, `public`, `private`, `protected`, `published`, `extend`, `switch`, `true`, `false`, `null`, `void`, `this`, `self`, `struct`, `char`, `signed`, `unsigned`, `short`, `long`.
