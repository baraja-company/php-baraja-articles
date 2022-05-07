Conditions en PHP - IF() {...} - options de branchement
=======================================================

> id: '3e4b81bb-a8bb-4e99-8842-e76f1658a371'
> slug:
> 	cs: if
> 	fr: conditions-en-php---if-...---options-de-branchement
> 
> perex: 'Podmínky, logické výrazy, booleovská logika a možnosti větvení PHP scriptů. Vyhodnocování logických výrazů a operátory. Možnosti skládání výrazu.'
> publicationDate: '2019-11-26 11:55:09'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '759a244b4fe2dc119c475aee42259578'

**Note : ** Cet article peut être un peu compliqué pour certains débutants car il suppose une compréhension de base de PHP. Si vous êtes intéressé par le fonctionnement des conditions, lisez <a href="/conditions">sur les conditions dans le cours pour débutants</a>.

| Support : Toutes les versions : PHP 4, PHP 5, PHP 7
|-------------------|----------|
| Brève description : | Validation d'une ou plusieurs déclarations
| Type : | <a href="/statements-and-functions">État, construction</a> (pas une fonction)

Description
-----

Souvent, vous devez déterminer si une égalité se vérifie ou si une déclaration est vraie, c'est à cela que servent les conditions. PHP utilise la syntaxe suivante comme beaucoup d'autres langages (notamment le C) :

```php
if (logický výrok) {
    // construire
}
```

Chaque expression logique a une valeur de `TRUE` (vrai) ou `FALSE` (faux), il n'y a pas d'autres options.

Exemple de comparaison pour savoir si la variable `$x` est plus grande que la variable `$y` :

```php
$x = 10;
$y = 5;

if ($x > $y) {
    // Cette partie du script sera exécutée si la condition est vraie.
} else {
    // Cette partie du script sera exécutée si la condition ne s'applique pas.
}
```

La construction de condition a un contenu obligatoire entre parenthèses rondes, dans lequel est donnée l'expression à tester, composée d'opérateurs (aperçu ci-dessous), plusieurs expressions peuvent être liées à l'aide d'opérateurs logiques (aperçu ci-dessous).

En outre, la condition contient deux blocs de déclaration facultatifs.

- Si la condition est remplie
- Si la condition n'est pas remplie

Pour des raisons pratiques, il faut toujours inclure au moins le premier bloc d'instructions lorsque la condition est vérifiée, sinon le test de l'expression n'aurait pas de sens.

Utilisation des points-virgules et des parenthèses
--------------------------

En général :
- Les parenthèses rondes** sont utilisées pour séparer les expressions logiques (d'autres parenthèses rondes peuvent être plongées pour obtenir des expressions plus complexes).
- Le crochet **complexe** est utilisé pour délimiter un bloc de commandes et de fonctions.
- Le **milieu** n'est pas utilisé pour indiquer une condition (le bloc de commandes est délimité par une parenthèse composée), mais pour séparer les commandes individuelles au sein de la condition).

La seule notation possible avec un point-virgule (sauf lors de l'utilisation de la construction **endif**) :

```php
if ($x > $y);
```

Cependant, une telle condition n'a pas de sens car, dans les deux cas, le résultat de la comparaison sera écarté et aucune instruction appartenant à la condition ne sera exécutée.

Notation alternative
--------------------------

Dans certaines circonstances, la construction `si` peut être utilisée avec l'omission des crochets composés. Cela ne peut être réalisé que dans les cas suivants :

- Si nous n'exécutons qu'une seule instruction dans la condition.
- Si nous utilisons un deux-points et un endif au lieu des crochets composés ;
- Si nous utilisons la notation "en ligne".

Pour des informations plus détaillées, voir les 3 chapitres suivants.

> **`1. une seule commande ~ syntaxe abrégée`**

Si vous créez une condition dans laquelle vous ne voulez exécuter qu'une seule construction (instruction), vous pouvez utiliser la notation classique des crochets composés :

```php
if ($x > 10) { $y = $x; }
```

Ou nous pouvons omettre les parenthèses :

```php
if ($x > 10) $y = $x;
```

Cependant, ce comportement ne s'applique qu'à une commande à proximité immédiate de la condition.

Un meilleur exemple (seule la construction `$y = $x` est exécutée conditionnellement, le reste est toujours exécuté) :

```php
$x = 5;
$y = 3;
$z = 10;
if ($x > $y)
$y = $x;
$x = 3;
```

> **`2. Colonne et endif;`**

```php
if (výraz):
    konstrukt;
    konstrukt;
    konstrukt;
endif;
```

Cependant, cette notation a longtemps été considérée comme obsolète car son orientation se réduit lorsque de multiples conditions sont immergées en elles-mêmes.

> **Note:** Je tiens à noter que ce style est également apprécié par certaines personnes, comme Yuh (<a href="https://www.jakpsatweb.cz/php/php-tahak.html#vetveni">voir son article</a>). Dieu interdit que tu utilises ça quelque part.

> **`3. Expression ternaire ~ notation "en ligne" à ligne unique`**

Il est parfois utile d'effectuer une simple comparaison en ligne avec une autre action (par exemple, avec la définition d'une nouvelle variable). Si nous voulons exécuter une seule instruction, toute la procédure peut être réduite à une seule ligne, tout en restant aussi simple que possible.

```php
$x = 5;
$xJeVetsiNezDva = ($x > 2 ? TRUE : FALSE);

// ou encore plus court :

$xJeVetsiNezDva = ($x > 2);
```

Tables d'opérateurs
-----------------

Deux types d'opérateurs sont utilisés dans la condition :

- **Comparatif** ~ ils comparent une relation spécifique,
- **Logique** ~ combine plusieurs expressions pour créer des conditions complexes.

Opérateurs comparatifs
-----------------------

| Opérateur | Signification |
|----------|----------|
| `==` | Égaux
| `===` | Egale et a le même type de données
| `!=` | N'est pas égal à lui-même
| `>=` | Egale ou est plus grande
| `<=` | Egale ou est inférieur
| `>` | Grand
| `<` | Moins

Exemple (valable lorsque `$x n'est pas 5`) :

```php
if ($x != 5) { ... }
```

Opérateurs logiques
--------------------

| Opérateur | Alternative | Signification | Vrai quand :
|-----------|--------------|----------------|--------------------------------------------
| ET | et en même temps | les deux valeurs sont vraies
| `||` | OR | ou | au moins une valeur est vraie
| `^^` | XOR | OU exclusif | au moins un est vrai ou faux, mais jamais les deux.
| `!` | *doesn't* | négation de l'expression | `true` quand `false` et vice versa
| La négation de l'expression "ne fait pas" dépend des circonstances.

Un exemple plus complexe :

```php
$x = 5;
$y = 3;
$z = 8;
if ($x > 0 && !($y != 2 && $z == $x) || $z > $y) { ... }
```

Omettre les opérateurs logiques et de comparaison
---------------------------------------------

Souvent, nous pouvons nous permettre d'omettre l'un ou l'autre opérateur (ou même les deux), mais nous ne devons jamais oublier les règles de bon usage pour que l'expression résultante fonctionne.

En général, lorsqu'on teste une expression sans opérateur, on teste si sa valeur est `TRUE` ou non vide (par exemple, elle contient un nombre non nul, une chaîne non vide, ...).

Exemples :

```php
$x = 5;
$y = 3;
$z = 8;

if ($x) { ... }         // passe car $x n'est pas vide
if ($x && $y) { ... }   // passe car $x et $y ne sont pas vides
if (!$x) { ... }        // échoue parce que TRUE est nié
if (isset($z)) { ... }  // passe car la variable $z existe
```

Mais des situations délicates peuvent survenir, notamment lorsque.. :
- Je demande `si ($x)` et la variable `$x` contient zéro (`0`), alors la condition n'est pas satisfaite.
- Ou bien la variable `$x` contient la chaîne ``0`` (le nombre zéro), car elle déborde sur zéro et l'expression n'est donc pas vraie.
- Il y a une fonction dans la condition qui renvoie toujours une chaîne de caractères non vide, car alors la condition est toujours vraie.
- Ou si nous vérifions l'entrée de l'utilisateur et qu'il renvoie `'false'` sous forme de chaîne, alors la condition est vraie car la chaîne est non vide.

Je recommande une solution simple et efficace à ce problème : demandez le nombre de caractères qui sont renvoyés. Si la chaîne est vide (ou si la variable n'existe pas), zéro caractère est renvoyé et la condition n'est pas satisfaite. Un exemple simple :

```php
$x = '0';
if ($x) { ... }			// la condition ne s'applique pas normalement
if (strlen($x)) { ... }	// la condition est valide car $x contient 1 caractère
```

Ensuite, nous pouvons tester l'existence d'une variable en utilisant la fonction `isset()`.

Comparaison de chaînes de caractères
-----------------

Il est facile de constater que les cordes sont identiques :

```php
$a = 'Chat';
$b = 'chat';

if ($a === $b) {
    // Si les chaînes sont les mêmes
} else {
    // Si les chaînes de caractères sont différentes
}
```

Il est important de garder un œil sur les types de données au cas où l'entrée pourrait être équivalente à une autre.

Par exemple, la chaîne vide `$a = '';` est différente de la chaîne `NULL` : `$b = NULL;`. Nous devons faire cette distinction, par exemple, à cause des bases de données où il y a une différence entre une valeur inexistante et une valeur vide.

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

Il est également judicieux d'ignorer les caractères blancs (invisibles) tels que les espaces, les tabulations et les sauts de ligne lors de la comparaison de chaînes de caractères. Cela est utile, par exemple, pour saisir un mot de passe et le transmettre à une fonction de hachage :

```php
$password = '81dc9bdb52d04dc20036dbd8313ed055'; // 1234
$userPassword = '1234';

if (md5(trim($userPassword)) === $password) {
    // La fonction trim() supprime automatiquement les espaces.
}
```

Valeur inconnue (inexistante) ?
--------------------------

Parfois il peut arriver que la valeur n'existe pas (elle n'est ni `TRUE` ni `FALSE`), il s'agit surtout d'une valeur obtenue de la base de données (par exemple, on demande une colonne qui n'existe pas), dans ce cas le type de données `NULL` sera retourné.

En général, `NULL` est évalué comme `FALSE`, c'est-à-dire que la condition ne s'applique pas. Cependant, ce comportement n'est pas toujours pratique, car une valeur inexistante ne signifie pas nécessairement qu'il n'y a pas d'enregistrement.

> Exemple tiré de la pratique : Nous avons un profil d'utilisateur et nous interrogeons la page web de l'utilisateur. Tous les utilisateurs n'ont pas besoin d'avoir une page web, donc dans ce cas `NULL` est retourné, mais l'utilisateur existe toujours. Donc dans ce cas, nous devrions plutôt utiliser la fonction `isset()` pour tester la (non-)existence de la variable et ne pas faire une conclusion basée sur une valeur spécifique.
