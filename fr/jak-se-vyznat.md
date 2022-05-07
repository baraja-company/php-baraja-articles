Comment comprendre le code PHP
==============================

> id: d8d452da-37b4-4502-a573-a27afe7ea37a
> slug:
> 	cs: jak-se-vyznat
> 	fr: comment-comprendre-le-code-php
> 
> publicationDate: '2020-04-11 18:45:23'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c3012a19e6dd01770fad9ab456a5b6b3

La plupart des langues peuvent être écrites de différentes manières, avec le même résultat. En même temps, une fois que vous avez écrit le code, vous allez probablement le relire un jour ou l'autre et le corriger ou ajouter de nouvelles fonctionnalités.

Par conséquent, pour éviter d'avoir à penser au code tout le temps et pour bien s'y retrouver, il existe un ensemble d'outils et de moyens pour "bien écrire le code" directement en PHP, ou pour construire le code d'une manière qui favorise directement sa lisibilité future (même par un autre humain).

> **Note de l'auteur:**
>
> L'expérience montre que le code devient obsolète si rapidement que même l'auteur de l'application lui-même perçoit son propre code comme étranger après six mois. Ainsi, si nous l'écrivons correctement dès le départ, cela n'empêchera pas son extensibilité future.
>
> Dans le développement réel, il est secrètement démontré qu'un formatage uniforme du code et l'introduction de règles en général permettent d'éviter un certain nombre d'erreurs.

TL;DR
-----

La lisibilité du code est souvent liée au formatage et aux règles d'écriture.

Au sein des équipes de développement, il est logique d'établir des règles formelles sur la façon dont nous formatons et maintenons le code.

Personnellement, j'utilise (en 2022) la norme de codage du framework Nette et les règles sont évaluées automatiquement dans chaque commit. Voir l'article sur [l'utilisation de GitHub CI] (https://php.baraja.cz/github-actions-nejlepsi-ci-pro-rok-2021#hotove-priklady) pour plus d'informations.

L'installation du test de la norme de codage et son exécution se font à l'aide d'une paire de commandes :

```shell
composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
php temp/coding-standard/ecs check src
```

Notes dans le code
---------------

Les notes n'ont aucun effet sur le traitement du code et sont à l'usage exclusif du programmeur. Pour les morceaux de code plus importants et plus complets, il est important d'écrire une note qui explique à quoi sert le code et comment il fonctionne en principe.

```php
// Définitions des variables
$a = 5;
$b = 3;
$c = 2;

// Somme de tous les nombres
$sum = $a + $b + $c;

// Liste des utilisateurs
echo $sum;
```

La note commence par une paire de barres obliques (`//`) et est valable jusqu'à la fin de la ligne. Il peut être utilisé partout.

La note ne doit pas expliquer la mise en œuvre spécifique de l'algorithme, mais plutôt ses principes généraux. En effet, le code peut changer plusieurs fois au fil du temps, auquel cas nous devons également corriger la note.

> **Note de l'auteur:**
>
> Il arrive souvent que le code ne fasse pas exactement ce que sa description explique. Cela est principalement dû au fait que le programmeur a fait une erreur quelque part. La note doit donc décrire les principes généraux afin que nous puissions ensuite modifier le code en conséquence. Mais n'oubliez jamais que la seule vérité sur ce qui se passe réellement dans une application est décrite uniquement par le code réel, et la note n'a aucun effet sur cela.

Séparation graphique des parties du code
----------------------------

Lors de la conception d'une application, il est important de séparer les blocs logiques les uns des autres. Habituellement, ils sont séparés en fonctions, en méthodes ou, dans le cas du code de base, au moins par des commentaires.

Dans un algorithme plus long, je décris généralement le principe général de l'algorithme au début, puis je numérote les différents endroits du code afin que le développeur puisse mieux comprendre la fonctionnalité spécifique basée sur ces endroits.

```php
/**
 * La fonction calcule la moyenne arithmétique.
 *
 * 1. Obtenir une liste de numéros
 * 2. Obtenir la somme et le nombre de nombres
 * 3. calculer et imprimer la moyenne
 */

// 1.
$numbers = [1, 3, 8, 12];

// 2.
$sum = array_sum($numbers);
$count = count($numbers);

// 3.
echo 'La moyenne est :' . ($sum / $count);
```

Les caractères `/**` commencent un commentaire de plusieurs lignes qui s'applique jusqu'au marqueur `*/`. Pour faciliter la lecture, il est bon de placer un astérisque au début de chaque ligne.

Commentaires sur la documentation
----------------------

Les commentaires de documentation sont généralement utilisés pour décrire et documenter les fonctions (comportement, paramètres, valeurs de retour, auteur, etc.).

Dans les versions antérieures de PHP (avant `7.0`), les types de données n'étaient pas encore utilisés, donc le type d'une variable particulière était décrit directement dans le commentaire.

```php
/**
 * @author Jan Barášek <jan@barasek.com>
 * @license MIT
 * @link https://php.baraja.cz
 * @param float[] $numbers
 */
function average(array $numbers): float
{
    $sum = array_sum($numbers);
    $count = count($numbers);

    return $sum / $count;
}
```

Les commentaires de documentation sont appelés "documentation" principalement parce qu'ils ont un format préétabli qui est compris par les environnements de développement spécifiques (et les éditeurs), mais aussi par les outils automatisés de génération de documentation ou de vérification du code.

Écrire du code en tchèque ou en anglais ?
-----------------------------

J'écris tout le code en anglais uniquement (y compris les noms de fonctions, les variables, les commentaires, ...).

Cela présente un certain nombre d'avantages :

- Le développeur peut former son anglais de manière proactive dès maintenant.
- Une grande partie de l'application utilise des bibliothèques tierces qui sont en anglais, ce qui permet de maintenir automatiquement la cohérence.
- La plupart des trucs avancés n'ont pas du tout de traduction anglaise.
- Je suis sûr que vous pouvez penser à de nombreux autres exemples.

Le PHP ne requiert pas directement l'anglais et vous pouvez tout écrire en anglais. Je vois l'utilisation de l'anglais plutôt comme une sorte d'investissement pour l'avenir, et une opportunité d'étendre facilement le code par d'autres personnes qui ne sont pas de langue maternelle anglaise.

Un code entièrement localisé en anglais est également utilisé dans les entreprises, il est donc bon de pratiquer l'anglais dès le début.

Ordre des opérations numériques
------------------------

Gardez toujours à l'esprit que PHP arrondit les nombres lors des opérations numériques. Cela peut souvent être gênant, car tout résultat comportant des chiffres décimaux s'accompagne d'une certaine imprécision.

Une bonne solution semble être d'incrémenter d'abord les nombres, puis de calculer avec les plus grands nombres possibles. De cette façon, il y a statistiquement moins de distorsion.

Exemple :

```php
echo 10 / 3; // Il écrit 3.3333333333333
```

Dans certains cas, vous pouvez également utiliser l'astuce consistant à ne pas utiliser de décimales du tout et à tout calculer comme un nombre entier. Dans ce cas, cette distorsion n'existe pas :

```php
echo 1 / 2 * 2; // c'est pire car 1/2 = 0,5*2 = 1
echo 2 * 1 / 2; // c'est mieux car 2*1 = 2/2 = 1
```

Lorsque vous résolvez des opérations numériques importantes et complexes, utilisez des fractions pour écrire les nombres.
