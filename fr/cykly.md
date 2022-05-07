Cycles et leurs types en PHP
============================

> id: e2927790-d0fb-4de5-9848-01fdd088464b
> slug:
> 	cs: cykly
> 	fr: cycles-et-leurs-types-en-php
> 
> perex: 'Cykl for, while, foreach a rekurze v PHP + vysvětlení rozdílů mezi cykly. Možnosti iterativního zpracování úloh.'
> publicationDate: '2020-04-11 18:56:34'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '297931d6e7b8c9e011dbbe06b414cdde'

Les boucles en général sont utilisées pour répéter la même section de code (généralement sur un ensemble de données). Pour chaque type de tâche, un type de boucle différent est adapté, qui diffère principalement par sa signification et sa syntaxe. Il est également important de noter que tous les types de boucles sont convertibles entre eux, mais que cela ne vaut pas toujours la peine en termes de complexité du code et de temps de calcul.

La division de base des boucles en termes de type d'élément :

- `for` : Une boucle générique où l'on connaît le nombre de répétitions à l'avance, ou peut simplement le calculer à l'avance,
- `while` et `until-while` : Une boucle générale où l'on ne connaît pas le nombre de répétitions à l'avance,
- `foreach` : Naviguer dans les tableaux et les objets,
- `recursion` : Décomposer un problème complexe en un ensemble de problèmes plus petits.

Propriétés générales des boucles
-----------------------

Les cycles fonctionnent généralement en répétant un morceau de code marqué **encore et encore**, **tant que la condition de terminaison** est maintenue. La boucle est arrêtée soit en ne satisfaisant pas la condition de terminaison, soit en l'arrêtant manuellement en appelant `break;`.

Si rien n'arrête la boucle, rien ne l'empêche de fonctionner indéfiniment, ou jusqu'à ce que l'application soit arrêtée manuellement, ou que le délai de traitement du script PHP soit écoulé (généralement 30 secondes).

**Termes importants:**

- `Cycle` - une expression du langage qui vous permet de répéter une certaine partie du code.
- Itération - une exécution spécifique du corps de la boucle.
- `Initialization` - le début de l'exécution de la boucle (avant que la première itération ne commence)
- `Increment` - incrémenter une variable d'itération par un
- Condition de sortie - Une condition qui est vérifiée à chaque itération (l'emplacement de la vérification varie selon le type de boucle). La boucle fonctionne tant que la condition est maintenue

Pour la boucle
----------

Le `Pour` est utile dans les cas où le nombre de répétitions est connu à l'avance (ou peut être facilement calculé). Il est parfait pour le parcours linéaire d'intervalles, par exemple.

Il est généralement rédigé (3 parties) :

```php
for (inicializace; výraz; inkrementace) {
    // le corps du cycle
}
```

- `Initialisation` : Définition de l'état initial de la boucle (quelles variables nous avons besoin),
- `expression` : Condition. Tant qu'il est valide, la boucle s'exécute,
- `increment` : Change l'état de la boucle depuis la passe précédente (`increment` - incrémenter par un).

Un exemple serait d'afficher une série de chiffres de 0 à 10 :

```php
for ($i = 0; $i <= 10; $i++) {
    echo $i . '<br>';
}
```

Ou des multiples de deux de `2` à `100` :

```php
for ($i = 2; $i <= 100; $i += 2) {
    echo $i . '<br>';
}
```

La boucle for est utile pour diverses astuces, comme <a href="/abeceda-cisla-intervals">obtenir l'alphabet, un tableau de chiffres et des intervalles</a>.

Une boucle do-while
-----------------------

Une boucle `While` fonctionne tant que la condition est maintenue. La différence entre `while` et `do-while` est le moment où la condition sera évaluée.

```php
while (výraz) {
    // le corps du cycle
}
```

Alternativement :

```php
do {
    // le corps du cycle
} while(výraz);
```

- Le `while` évalue d'abord la condition et exécute ensuite la boucle interne,
- Le `do-while` traite d'abord la boucle interne et évalue ensuite la condition.

C'est particulièrement avantageux lorsque l'on ne peut pas savoir à l'avance si la boucle s'exécutera jamais et que l'on veut garantir qu'elle s'exécute toujours au moins une fois (on utilise alors `do-while`).

Exemple :

> Nous avons un nombre stocké dans la variable `$x = 2`. Nous voulons également générer un nombre dans l'intervalle `<0 ; 10>` dans la variable `$y` afin qu'il soit différent du nombre dans la variable `$x`. Pour faire simple : générez un nombre aléatoire entre 0 et 10 qui n'est pas égal à 2.

Dans ce cas, il est pratique d'utiliser une boucle `do-while`. Nous ne savons pas à l'avance combien de fois la boucle devra s'exécuter (nous pouvons être malchanceux et tomber sur le même nombre, auquel cas la boucle s'exécutera pendant longtemps), et nous voulons également garantir qu'elle s'exécutera au moins une fois avant que la condition ne soit évaluée, puisque nous devons d'abord générer le nombre.

```php
$x = 2;
$y = $x;

do {
   $y = rand(0, 10);
} while($x == $y);

echo 'X :' . $x . '<br>';
echo 'Y :' . $y;
```

Foreach
-------

`foreach` est parfait pour parcourir les champs et les objets. Nous pouvons obtenir non seulement des valeurs spécifiques, mais aussi des clés.

Si nous voulons seulement obtenir des valeurs spécifiques :

```php
foreach ($data as $value) {
    // traitement des données
}
```

Sinon, nous pouvons obtenir les clés :

```php
foreach ($data as $key => $value) {
    // traitement des données
}
```

`foreach` est parfait pour parcourir des données réelles - par exemple, les résultats d'une base de données, ou des champs avec des clés :

```php
$countries = [
    'sur' => 'République tchèque',
    'Voir' => 'Slovaquie',
];

foreach ($countries as $shortcut => $name) {
    echo $shortcut . ':' . $name . '<br>';
}
```

Récursion
-------

La récursion se produit lorsqu'une fonction ou une méthode s'appelle elle-même. Elle permet de décomposer un problème complexe en un ensemble de problèmes plus petits.

> La compréhension de la récursion peut s'avérer difficile pour les débutants, car elle est basée sur l'idée du transfert de responsabilité.
>
> La fonction dit en fait quelque chose comme "Je ne peux pas résoudre ce problème, mais je connais quelqu'un qui le peut...", donc elle l'appelle, il appelle quelqu'un, ... jusqu'à ce que le membre final soit appelé, et il coupe le problème.

Il convient de noter que tout algorithme récursif peut être réécrit pour utiliser des boucles là où la récursion n'est pas nécessaire (l'inverse est également vrai). La récursion est un bon serviteur, mais un mauvais maître. Elle permet de résoudre certains types de problèmes de manière simple et très efficace, tandis que le bouclage des cycles est utile pour d'autres choses.

En général, la récursion est gourmande en mémoire car elle crée sans cesse de nouvelles instances et de nouveaux contextes de fonctions et de méthodes. Cependant, pour traverser des structures arborescentes, par exemple, c'est la seule façon raisonnable de résoudre le problème.

Exemple :

Nous devons calculer la factorielle du nombre `5`, c'est ainsi que cela se fait en mathématiques :

`5 ! = 5 * 4 * 3 * 2 * 1`

C'est-à-dire qu'il y a une multiplication successive d'une série de nombres à partir de `1` jusqu'au nombre qui nous intéresse, la factorielle de.

De manière récursive, cela peut être résolu de manière très élégante :

```php
function factorial(int $n): int
{
   if ($n === 0) {
      return 1;
   }

   return $n * factorial($n - 1);
}
```

Une autre solution encore plus courte consiste à utiliser l'opérateur ternaire :

```php
function factorial(int $n): int
{
    return $n === 0 ? 1 : $n * factorial($n - 1);
}
```

La même chose peut être réécrite pour utiliser les cycles :

```php
function factorial(int $n): int
{
    $result = 1;

    for ($i = $n; $i > 0; $i--) {
        $result *= $i;
    }

    return $result;
 }
```

L'écriture avec un cycle est légèrement plus longue, mais l'empreinte mémoire est nettement plus faible. Nous n'utilisons qu'une seule variable `$result` pour le calcul, dont nous changeons la valeur continuellement et ne devons pas créer de nouvelles instances de `factorial()`.

Écrivez les boucles infinies très soigneusement
-----------------------------------

Il peut être très facile pour une boucle de devenir infinie (nous y parvenons en ne satisfaisant jamais la condition de terminaison). Vous devez en tenir compte et éventuellement arrêter la boucle vous-même avec `break;`.

Par exemple, lancez le dé jusqu'à ce que le nombre "6" soit obtenu. L'implémentation est tentée d'utiliser la boucle `while` :

```php
while (true) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'La valeur a chuté' . $value;
      break;
   }
}
```

En écrivant `while(true)`, nous avons assuré un nombre infini de répétitions, car `true` sera toujours vrai. Nous devons donc arrêter la boucle nous-mêmes avec la construction `break;`, mais que faire si la valeur `6` ne tombe jamais et que nous n'arrêtons jamais la boucle ?

Personnellement, je compte toujours le nombre de répétitions et s'il dépasse une certaine limite critique, je termine la boucle de force. Par exemple, si nous dépassons les 1000 tentatives. Dans ce cas, il est préférable d'utiliser `for` plutôt que `while` et de compter le nombre de passages.

Si nous dépassons le nombre d'exécutions, il est poli d'en informer le développeur en lançant une exception.

```php
for ($i = 0; $i <= 1000; $i++) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'La valeur a chuté' . $value;
      break;
   }
}

if ($i === 1000) {
   throw new \Exception('Le nombre maximal de lancers a été dépassé.');
}
```
