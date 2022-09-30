Opérateurs ternaires en PHP (? :) - condition en une ligne
==========================================================

> id: '1191be9b-ff9d-4725-b0b4-17b60de2b935'
> slug:
> 	cs: ternarni-operator
> 	fr: operateurs-ternaires-en-php-condition-en-une-ligne
> 
> perex:
> 	- Ternární operátor umožňuje zapsat jednoduchou podmínku do jednoho řádku jako výraz.
> 	- L'opérateur ternaire vous permet d'écrire une condition simple sur une ligne sous forme d'expression.
> 
> publicationDate: '2019-11-26 11:59:18'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: '20ff690401289869eae2a2584fae7568'

L'opérateur ternaire vous permet de raccourcir une condition simple en une seule ligne à un endroit où l'analyse syntaxique est inutile, complexe ou carrément inappropriée.

TL;DR
------

Les opérateurs ternaires sont des raccourcis pour les conditions "if" et "else", vous n'avez donc pas besoin de les utiliser. Ils sont utiles pour les éléments de logique de vérification qui se répètent sans cesse. Utilisez toujours le format `(condition ? logique positive : logique négative)` incluant des parenthèses. Utiliser pour une vérification courte afin de rendre le code plus clair et plus court.

Définitions de base
------------------

Souvent, nous avons une condition de la forme, par exemple :

```php
$a = 5;
$b = 3;

if ($a > $b) {
     echo 'C'est plus grand';
} else {
     echo 'C'est plus petit';
}
```

Ainsi, si nous voulons écrire une seule phrase simple, nous devons utiliser 4 lignes de code, qui pourraient être réduites à une seule.

```php
$a = 5;
$b = 3;

echo 'Il est' . ($a > $b ? 'plus grand' : 'plus petit');
```

En général, l'opérateur ternaire est écrit en 3 parties (c'est pourquoi il est appelé "ternaire") :

```php
(condition ? 'oui' : 'de')
```

Les opérateurs ternaires sont très souvent utilisés dans la pratique, par exemple pour désigner les lignes paires d'un tableau :

```php
$pole = [3, 1, 4, 1, 5, 9, 2];

for ($i = 0; $pole[$i]; $i++) {
     echo '<tr class=""' . ($i % 2 ? 'de' : 'même') . '">';

          // C'est en quelque sorte travailler avec un tableur...
          // par exemple : echo '<td>' . $field[$i] . '</td>' ;

     echo '</tr>';
}
```

Exemple d'utilisation de l'opérateur ternaire
------------------------------------

Très souvent, nous devons vérifier l'existence de la valeur d'une variable et l'utiliser immédiatement si nécessaire. S'il n'existe pas, nous voulons renvoyer la valeur par défaut.

L'approche classique consiste à procéder comme suit :

```php
$a = 5;
$b = 8;

$c = $a ? $a : $b;
```

Si la variable `$a` existe, utilisez `$a` comme valeur, sinon `$b`.

Cependant, il arrive que nous obtenions la valeur à partir d'une fonction :

```php
$a = 5;
$b = 3;
$default = 42;

$c = my_function($a, $b) ? my_function($a, $b) : $default;
```

Cette méthode d'appel est très inefficace en termes de ressources système. D'abord, la fonction doit être appelée, et si elle existe, elle est appelée à nouveau pour obtenir la valeur, qui est stockée dans la variable `$c`.

Cela pourrait être mieux géré via une variable d'aide :

```php
$a = 5;
$b = 3;
$helper = my_function($a, $b);
$default = 42;

$c = $helper ? $helper : $default;
```

Utilisation inappropriée
------------------

L'opérateur ternaire ne vaut pas toujours la peine d'être utilisé car il a tendance à prêter à confusion lors de l'écriture de conditions compliquées et imbriquées.

Voyez par vous-même un exemple :

```php
$valid = true;
$lang = 'Français';

$x = $valid
     ? ($lang === 'Français' ? 'oui' : 'oui')
     : ($lang === 'Français' ? 'non' : 'de');
```

Sauriez-vous au premier coup d'œil que la variable `$x` contient la valeur `oui` ?

Après un peu de pratique, vous pourriez, mais ce n'est pas la bonne réponse. :)

Vérification de la (non-)existence de la valeur et utilisation de la valeur par défaut
--------------------

Les opérateurs ternaires sont les plus puissants dans la vérification de routine de la (non-)existence des valeurs et l'utilisation d'autres valeurs par défaut.

Par exemple, nous voulons vérifier si une catégorie principale existe pour un article et, si ce n'est pas le cas, produire un message de remplacement :

```php
echo $mainCategory ?? 'La catégorie n'existe pas';
```

L'opérateur `??` (deux points d'interrogation) vérifie si la variable `$mainCategory` existe et n'est pas `null`. Elle fonctionne de la même manière que la fonction `isset()`.

Valider la vacuité d'une valeur
-----------------------------

Une construction très souvent utile pour vérifier que la valeur de sortie n'est pas vide (c'est-à-dire pas `null`, `0`, `false` ou `''*(chaîne vide)*).

On peut l'écrire comme suit :

```php
$a = 5;
$b = 3;
$default = 42;

$c = my_function($a, $b) ?: $default;
```

D'abord la `fonction($a, $b)` est appelée, puis sa valeur est testée et si elle n'est pas vide, elle est immédiatement passée à la variable `$c`, sinon la variable `$default` est utilisée.

L'opérateur `?:` fonctionne comme l'opérateur `!=` dans une conditionnelle (faux quel que soit le type de données), et on peut s'en souvenir en ressemblant à un smiley avec des cheveux Elvis, par exemple.

Prise en charge des anciennes versions de PHP
----------------------------

L'opérateur `?:` fonctionne depuis PHP 7. Dans les versions plus anciennes, nous devons nous contenter de la condition `if (...)`, qui permet d'obtenir le même comportement. Rappelez-vous que les opérateurs ternaires ne sont en fait qu'un raccourci pour écrire la même chose que celle qui est traitée par les conditions.

La non-existence d'une valeur peut être vérifiée avec la fonction `isset()`, qui vérifie que la variable existe et n'est pas vide (`null`).

Au lieu du code :

```php
$a = 5;
$b = 3;

$c = $a ?? $b;
```

Ensuite, nous écrivons l'alternative la plus ancienne :

```php
$a = 5;
$b = 3;

$c = isset($a) && $a ?? $b;
```

> **Avertissement:** L'ordre de `isset()` et de la variable elle-même compte. Si nous inversions l'ordre et que la variable n'existait pas, une erreur d'accès à la variable inexistante serait levée.
