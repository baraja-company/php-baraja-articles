Variables en PHP
================

> id: b89774e7-143c-4a8c-8dc6-3b3d2c78d5b7
> slug:
> 	cs: promenna
> 	fr: variables-en-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '23e4d537f18a3b2e1946c79be1aacf52'

Cette page est un résumé complet du fonctionnement des variables en PHP. Le texte est rédigé dans un style légèrement technique et peut ne pas être entièrement compris par les débutants. Si vous êtes intéressé par les bases complètes, lisez alors <a href="/first-script">le tutoriel du débutant</a> et <a href="/principles-of-prominent-script">principes d'écriture des variables</a>.

Description
-----

Une variable est un emplacement virtuel dans la mémoire opérationnelle, elle est définie par `name` et `data type`. Dans le type de données, la variable a ensuite un certain "contenu".

En interne, PHP représente les variables comme une table de hachage, c'est à dire que toutes les variables sont stockées dans une table qui est très rapide à rechercher, donc le temps nécessaire pour accéder à chaque variable est *presque* constant.

Exemples d'écriture :

```php
$a = 10;
$b = 'chat';
$c = true;
```

Chaque ligne de l'échantillon correspond à la définition d'une variable. Le nom de chaque variable commence par le signe dollar `$` suivi du nom lui-même. Le signe égal peut être utilisé pour attribuer une valeur à la variable.

En interne, les variables seront stockées en mémoire sous forme de table de hachage :

| Nom | Type | Abréviation | Valeur | Nom.
|-------|---------|---------|---------|
| `$a` | integer | int | `10` |
| `$b` | chaîne de caractères | chaîne de caractères | `cats` | chaîne de caractères
| `$c` | boolean | bool | `true` |

Types de variables
---------------

Les variables sont classées en fonction des droits d'accès et de l'utilisation :

- <a href="/local-variable">variables locales</a> (valables dans leur contexte, c'est-à-dire au sein des fonctions et des méthodes),
- <a href="/global-variable">variables globales</a> (valables dans tout le script),
- <a href="/superglobal-variable">Variables superglobales</a> (valables dans l'ensemble de l'environnement du serveur - généralement des données utilisateur),
- <a href="/promenna-variable">variables</a> (une variable spéciale qui contient une référence (lien) à une autre variable).

* Les "variables globales" et les "variables variables" ne doivent pas être utilisées car elles contribuent à l'illisibilité du code et au comportement "magique" (inattendu) des applications.

Contenu autorisé des variables
--------------------------

Les variables peuvent contenir tout ce que leur type de données actuel autorise. Si le type de données n'est pas spécifié, il sera déterminé automatiquement, sur la base du contenu (ce qui n'est pas recommandé, car cela peut entraîner des erreurs).

Les types de données fonctionnent de manière indépendante, de sorte que nous pouvons utiliser presque tous les types. Cependant, si nous voulons effectuer une opération de fusion, nous devons toujours assurer la conversion vers un seul format.

Un bon exemple est celui de l'addition et de la multiplication des nombres :

```php
$x = 5;       // nombre entier
$y = 3;       // nombre entier
$z = $y + $y; // la variable $z sera composée à partir de plusieurs variables
```

Dans ce cas, PHP est confronté à la question de savoir quel type de données aura la variable `$z` nouvellement créée. S'il s'agit du même type de données et que l'opération est possible, le type de données est hérité.

Cependant, il arrive que nous puissions effectuer une opération avec plusieurs types de données :

```php
$x = 1;       // nombre entier
$y = 3.14159; // flotteur
$z = $y + $y; // flotteur
```

Dans ce cas, nous fusionnons integer et float. La sortie sera un nombre décimal, donc on utilise float. Dans ce cas, PHP va faire quelque chose appelé **repartitionnement dynamique**.

Cependant, nous ne pouvons pas toujours compter sur ce comportement. Par exemple, comment voulez-vous fusionner un nombre et une chaîne de caractères ?

```php
$x = 256;     // nombre entier
$y = 'Hé ! Hé !'; // flotteur
$z = $y + $y; // ? ??
```

Types de données (aperçu des plus importants)
--------------------------------------

PHP est un langage interprété, il a donc quelques particularités par rapport aux autres langages de programmation, l'une d'entre elles est que nous n'avons pas à spécifier de types de données pour les variables, c'est-à-dire que chaque variable change dynamiquement de type de données en fonction de son contenu (sauf indication contraire). Néanmoins, il est bon de connaître au moins les types de données de base, ce qui est particulièrement utile pour optimiser les applications ou travailler avec des bases de données.

La notation peut ressembler à ceci :

```php
$x = (int) 25; // crée une variable de type entier
```

<a href="/datove-typy">Vue d'ensemble des types de données</a>.

Héritage des types de données
-----------------------

Quel type de données aura la variable `$x` si nous ne connaissons que ce morceau de code source ?

```php
$x = $y;
```

Il dépend du type de données de la variable `$y` dont la valeur et son type de données seront hérités. Dans ce cas, nous ne connaissons pas la variable `$x`, donc nous ne pouvons pas continuer à évaluer le code et un message d'erreur serait envoyé.

Remplacement dynamique
---------------------

Ayons les 2 variables suivantes :

```php
$x = 10;
$y = '10';
```

Quelle est la différence entre le contenu des variables `$x` et `$y` ?

La variable `$x` est un nombre, `$y` est une chaîne (contenant un "1" et un "0"), donc si nous stockons simplement la variable en mémoire et n'effectuons aucune opération qui affectera la valeur. Par exemple, les 2 entrées suivantes donneront le même résultat :

```php
echo $x + 5;	// imprime 15
echo $y + 5;	// imprime 15
```

Dans le second cas, ce qu'on appelle **l'écrasement dynamique** se produit, c'est-à-dire que la variable convertit son type de données de sorte qu'une opération de calcul puisse être effectuée sur elle. On ne peut pas toujours se fier à ce comportement et il s'agit plutôt d'un comportement correctif destiné à corriger les scripts de débutants mal écrits. Si possible, écrivez toujours les nombres avec un type de données pour les stocker, car cela augmente leur précision et les rend plus faciles à utiliser à l'avenir.

> **Note:** Il est important de noter que nous ne pouvons pas convertir les types de données de manière complètement arbitraire, ce qui n'est donc pas toujours possible. Si vous remplacez un type de données par un autre (incompatible), il se peut que la conversion ne se fasse pas du tout, ou que le contenu original soit corrompu ou complètement détruit et remplacé par un autre. Par exemple, si vous réécrivez une chaîne de caractères en un nombre entier (et qu'un texte qui n'est pas un nombre est stocké dans la variable), la valeur `1` sera stockée dans la variable au lieu de la valeur numérique.

Représentation des chaînes de caractères en tant que tableaux
------------------------------

Toutes les chaînes de caractères sont stockées en interne sous la forme d'un tableau de caractères. C'est-à-dire que chaque caractère a son propre **index** et peut être référencé. Si nous ne spécifions pas d'index, la chaîne entière est traitée.

```php
$x = 'Programmons en PHP !';
$n = 3;

echo $x;		// ceci imprime le contenu entier de la variable $x
echo $x[0];		// ceci imprime le caractère nul de la variable $x
echo $x[$n];	// ceci imprime le nième caractère de la variable $x
```

**Note : ** Les nombres PHP à partir de zéro, c'est-à-dire que le caractère zéro est 'P' et le premier caractère est 'r'.
>
> En outre, les caractères sont commutés par octet. Par exemple, le caractère "non" dans l'encodage UTF-8 est long de 2 octets, donc l'index du caractère dans la chaîne ne correspondra pas à la position réelle lors du défilement et 2 index seront utilisés pour stocker le caractère.

L'existence d'un élément de tableau doit toujours être vérifiée avec la fonction `isset()` :

```php
if (isset($x[$n])) {
    echo $x[$n];
}
```

Vous pouvez aussi l'écrire joliment avec un opérateur ternaire :

```php
echo $x[$n] ?? '';
```

Copie des variables
---------------------

Ayons la variable suivante :

```php
$q = 'Lorem ipsum, ...';
```

Et ensuite copier sa valeur dans la variable suivante :

```php
$qi = $q;
```

Heureusement, aucune copie ne sera effectuée et PHP stockera simplement une référence à la valeur dans une **table de hachage**. La valeur ne sera réellement copiée que lorsque la valeur de l'une des variables doit changer. Ce comportement est géré par un composant généralement appelé **collecteur Garbridge**.
