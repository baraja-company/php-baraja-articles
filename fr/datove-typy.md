Types de données en PHP
=======================

> id: cd63818c-259a-484e-b006-86c35b362019
> slug:
> 	cs: datove-typy
> 	fr: types-de-donnees-en-php
> 
> perex: 'Datové typy v PHP: String, integer, float, boolean, pole (array), objekt a další.'
> publicationDate: '2019-08-23 15:44:28'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: fcb2c647973c5f94e601c789b0a882f1

Toutes les données traitées en PHP sont d'un certain type. Par exemple, un nombre entier, une chaîne de caractères ou un booléen (vrai/faux).

Types de données de base
--------------------

Les types de base sont également appelés **types de données primitifs**, ou <a href="/fonction-is-scalar">types scalaires</a>.

| Type | Nom | Description
|---------|-----------------|-------|
| `int` | Integer | (`integer`) Ne contient que des entiers. La valeur maximale est déterminée par le nombre de bits. En PHP 32 bits, l'intervalle est de `-2 147 483 648` à `-2 147 483 647` (~ ± 2 milliards), en PHP 64 bits l'intervalle est de `-9 223 372 036 854 775 808` à `-9 223 372 036 854 775 807` (~ ± 9 quintillions). La valeur maximale peut toujours être obtenue en appelant la constante `PHP_INT_MAX`. Si la valeur maximale d'un entier est dépassée, PHP représentera le nombre comme `float` et l'écrasera automatiquement.
| `float` | Floating Point Decimal | Il s'agit d'une variante de nombre à virgule flottante pour laquelle la règle *"plus c'est petit, plus c'est précis "* s'applique. Le nombre est stocké en interne sous la forme de ce que l'on appelle une **mantisse** et un **exposant**, de sorte que vous stockez en fait 2 nombres entre lesquels l'opération `mantisse * (2^exposant)` est effectuée, ce qui permet de stocker une gamme de nombres vraiment énorme. Cette méthode utilise le principe selon lequel, pour les grands nombres, nous n'avons pas toujours besoin de connaître leur valeur exacte, mais nous voulons économiser autant de mémoire que possible. Les nombres de type `float` ne doivent pas être stockés exactement et ne doivent pas être utilisés pour calculer de l'argent.
| `string` | Chaîne | Contient une séquence de caractères délimités par des guillemets ou des apostrophes. La longueur maximale n'est limitée que par la capacité de la mémoire d'exploitation. Une chaîne peut être stockée dans n'importe quel codage, contenir des emoji ou des données binaires.
| `bool` | Boolean | (`boolean`) Valeur booléenne issue de l'algèbre booléenne, ne pouvant contenir que `true` ou `false`.
| La valeur vide `null` est utile dans les cas où l'on veut exprimer que quelque chose n'existe pas. Par exemple, un article n'a pas de catégorie. Parfois, `null` est incorrectement substitué à null (`0`) ou à des chaînes vides (`''`), mais ce n'est pas une bonne solution pour exprimer la non-existence.

**Attention:** Le type `null` n'est pas scalaire.

Zéro (0) vs. nul
----------------

De nombreux développeurs ont du mal à comprendre la différence entre `0` (zéro) et `null` (une valeur inexistante) lorsqu'ils commencent le développement.

Cette distinction peut être expliquée très bien et de manière humoristique avec l'image suivante :

<img src="{$baseUrl}/images/0-vs-null.jpg" alt="0 vs. null" class="w-100 mb-3">

Reconditionnement manuel
--------------------

Certains types peuvent être convertis entre eux. Le type de données cible est spécifié à l'aide de parenthèses rondes et peut être spécifié n'importe où dans l'application, par exemple lors de l'énumération d'une valeur.

Par exemple :

```php
$pi = 3.14;

echo $pi;       // imprime 3.14

echo (int) $pi; // imprime 3
```

Remplissage dynamique
---------------------

Considérons les 2 variables suivantes :

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

Dans le second cas, ce qu'on appelle **l'écrasement dynamique** se produit, c'est-à-dire que la variable convertit son type de données de sorte qu'une opération de calcul puisse être effectuée sur elle. On ne peut pas toujours se fier à ce comportement, et il s'agit plutôt d'un comportement correctif destiné à corriger les scripts de débutants mal écrits. Si possible, écrivez toujours les nombres avec un type de données pour les stocker, car cela augmente leur précision et les rend plus faciles à utiliser à l'avenir.

> **Note:** Il est important de noter que nous ne pouvons pas convertir les types de données de manière complètement arbitraire, ce qui n'est donc pas toujours possible. Si vous remplacez un type de données par un autre (incompatible), il se peut que la conversion ne se produise pas du tout, ou que le contenu original soit corrompu ou complètement détruit et remplacé par un autre. Par exemple, si vous réécrivez une chaîne de caractères en un nombre entier (et qu'un texte qui n'est pas un nombre est stocké dans la variable), la valeur `1` sera stockée dans la variable au lieu de la valeur numérique.

Comparaison des types
----------------

Lorsque vous comparez des valeurs, vous devez tenir compte des différents types de données.

En général, l'opérateur `==` est utilisé pour la comparaison générale de deux valeurs (indépendamment des types), tandis que `===` compare à la fois les valeurs et le type de données.

Par exemple :

```php
$a = '';
$b = null;

if ($a == $b) {
    // Il sera évalué comme VRAI parce que
    // le type de données est converti.
}

if ($a === $b) {
    // Effectue une validation beaucoup plus rigoureuse
    // et ça ne passera pas parce que c'est une différente
    // contenu et un type de données différent, donc
    // ce code ne sera jamais exécuté.
}
```
