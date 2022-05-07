Commandes, mots-clés et fonctions en PHP
========================================

> id: '96a00928-4410-479d-ada0-612de21cdacd'
> slug:
> 	cs: prikazy-a-funkce
> 	fr: commandes-mots-cles-et-fonctions-en-php
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '4534cc03d2ae66454b9f398139f232a4'

Chaque script PHP est composé de commandes et de fonctions, l'ensemble étant appelé **constructions**. Lorsque le script est traité, ces expressions linguistiques sont suivies (ce processus est appelé **tokénisation**) et, sur la base des *mots-clés*, l'interprète décide comment le processeur doit se comporter.

Commandes
--------------------------

Les commandes (<a href="https://www.php.net/manual/en/reserved.keywords.php">elles sont appelées **keywords** en anglais</a>) sont des parties déjà préprogrammées du langage, elles sont réservées spécifiquement à leur usage, peuvent être utilisées immédiatement dans n'importe quelle situation (elles ne nécessitent pas de bibliothèques spéciales supplémentaires ou d'installation), et constituent la base de tous les scripts.

Par exemple, <a href="/echo">extraction d'une chaîne de caractères en code HTML</a> :

```php
echo 'Echo est une commande linguistique permettant de lister le contenu.';
```

Avec les commandes, il est important de noter qu'elles ne renvoient aucune valeur et ne peuvent donc pas être utilisées comme une variable. Les commandes peuvent également être identifiées par le fait qu'elles ne contiennent pas de parenthèses.

Exemples :

```php
echo 'Bonjour, le monde !';
echo ('Bonjour, le monde !');
```

Si je mets le contenu de la commande entre parenthèses, le comportement ne changera pas et elle sera considérée comme une **expression**. C'est la même chose que si nous écrivions en mathématiques :

```php
5 + 3

// ou :

(5 + 3)
```

Techniquement, il y a une différence, mais dans la pratique, rien ne change.

Fonctions
--------------------------

S'il n'y avait que des commandes, ce serait plutôt ennuyeux. Les fonctions sont une collection de commandes multiples.

Nous voulons souvent exécuter le même code à plusieurs endroits de manière répétée. Dans ce cas, la copie constante représenterait trop de travail et pourrait entraîner des erreurs. Nous enveloppons donc ce code dans une fonction que nous nommons, puis nous l'appelons simplement par son nom.

Lorsque nous appelons la fonction, nous lui passons les paramètres comme variables, elle exécute le code et renvoie le résultat, que nous pouvons utiliser par la suite.

```php
$text = 'PHP est mon langage préféré !';

echo 'Texte "' . $text . '"est une longue' . strlen($text) . 'des personnages.';
```

PHP possède de nombreuses fonctions directement prédéfinies (<a href="/documentation">voir la documentation pour une liste complète</a>), mais il est souvent pratique de définir les vôtres :

```php
function mojeFunkce(int $x, int $y): int
{
   $z = $x + $y;   // ajoute les paramètres d'entrée et les stocke dans une variable

   return $z;      // renvoie la variable $z
}

echo mojeFunkce(5, 3); // imprime le nombre 8, car les nombres ont été traités par la fonction
```

Variables dans les fonctions
--------------------------

Les variables locales sont utilisées à l'intérieur des fonctions, ce qui signifie qu'elles ne peuvent être utilisées qu'à l'intérieur de la fonction et ne peuvent pas être manipulées (ou définies) en dehors de la fonction. Ils obtiennent leurs valeurs initiales à partir des paramètres de la fonction directement dans la définition de la fonction.

Exemple :

```php
$z = 5;

function prvniFunkce(int $x, int $y): int
{
   return $x - $y; // cela renvoie la différence entre les chiffres
}

function druhaFunkce(): mixed
{
   return $z; // ceci renvoie une erreur car la variable $z
              // non défini dans la fonction
}
```

Il est parfois utile de définir certains paramètres comme facultatifs, en définissant une autre valeur (par défaut) :

```php
echo prictiCislo(5);    // renvoie 6
echo prictiCislo(5, 7); // renvoie 12

function prictiCislo(int $x, int $y = 1): int
{
   return $x + $y;
}
```

La fonction `prictiCislo()` ajoute la valeur de la variable `$y` à la variable `$x`. Si la variable `$y` n'est pas définie (spécifiée comme paramètre lors de l'appel de la fonction), sa valeur par défaut écrite dans la définition de la fonction sera utilisée. Le deuxième paramètre de la fonction (la variable `$y`) est donc facultatif.

> Chaque fonction ne peut avoir qu'une seule sortie (un retour), si vous spécifiez plusieurs sorties dans une fonction, celle spécifiée en premier sera retournée. Si vous voulez renvoyer plusieurs valeurs, vous devez utiliser un tableau.
>
> En PHP (contrairement à d'autres langages), il n'est pas nécessaire de spécifier un retour. Dans ce cas, la fonction ne renvoie rien (void) quelle que soit l'entrée. Ces fonctions sont principalement utilisées pour stocker des données ou pour produire un code source. En général, cependant, il est recommandé de toujours renvoyer une valeur, au moins un accusé de réception indiquant que tout s'est bien passé.
