Signification particulière des guillemets
=========================================

> id: '2649942d-94d6-48c3-9c78-5a303165bf72'
> slug:
> 	cs: uvozovky-vyznam
> 	fr: signification-particuliere-des-guillemets
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c1b63b98a3e9d9c642247c363747616a

En PHP, vous pouvez souvent remplacer les guillemets par des apostrophes et obtenir le même effet. Il arrive même que nous utilisions volontairement une combinaison de guillemets et d'apostrophes pour obtenir un résultat différent sans avoir à utiliser l'échappement.

**TLDR : L'utilisation des guillemets peut être dangereuse dans certains contextes, utilisez donc les apostrophes partout à la place. Les guillemets ne conviennent que pour des cas particuliers.**

| Caractère | Nom
|------|-----------
| "`"` | guillemets |
| `'` | Apostrophe |

Premier exemple - même résultat
-----------------------------

```php
echo "Bonjour, le monde !"; // c'est la même chose
echo 'Bonjour, le monde !'; // comme ceci
```

Dans ce cas, l'utilisation des guillemets ou des apostrophes n'a pas d'importance. Ainsi, à première vue, il peut sembler qu'il n'y ait aucune différence entre les guillemets et les apostrophes.

Combinaison de guillemets et d'apostrophes
------------------------------

Considérons le code suivant :

```php
echo 'Ma couleur préférée est le "rouge" !';
```

Si j'utilisais des "retours chariot" pour délimiter la chaîne de caractères en cours d'écriture, il y aurait **une ambiguïté** entre le début et la fin de la chaîne. J'ai donc utilisé des "apostrophes" pour délimiter la chaîne, ce qui résout le problème.

Insertion de guillemets dans une chaîne de caractères délimitée par des guillemets
---------------------------------------------------

De temps en temps, il peut y avoir des situations où j'ai besoin de lister à la fois `quote` et `apostrophe` dans la même chaîne. Vous pouvez utiliser non seulement la concaténation de deux chaînes de caractères, mais aussi ce que l'on appelle le **escaping** des caractères.

```php
echo "Cette chaîne contient des guillemets.";
```

Dans ce cas, la barre oblique inversée a la signification "exactement ce caractère" et, par conséquent, le guillemet ne sera pas considéré comme la fin d'une chaîne (ou de quoi que ce soit d'autre) et sera donc toujours un guillemet. D'autres caractères étranges peuvent être marqués de cette manière lorsque leur durabilité ne peut être garantie et que la signification correcte pourrait être mal comprise.

Variable dans une chaîne de caractères
-----------------------

Les guillemets peuvent être extrêmement délicats car ils vous permettent d'insérer des variables directement dans une chaîne de caractères :

```php
$color = 'Rouge';

echo "Moje oblíbená barva je {$color}, a tvoje?";
```

Les apostrophes sont beaucoup plus sûres, car elles ne le permettent pas et vous devez plier la ficelle :

```php
$color = 'Rouge';

echo 'Ma couleur préférée est' . $color . 'et le vôtre ?';
```

Les bonnes habitudes
--------------------------

En général, je recommande d'utiliser les apostrophes (si possible) pour délimiter quoi que ce soit, car elles sont beaucoup moins courantes dans les chaînes de caractères que les simples guillemets.

De plus, PHP est un langage web, c'est-à-dire qu'il est utilisé pour générer des documents HTML, où les guillemets sont très courants précisément parce qu'ils sont aussi utilisés pour générer des parties du code HTML. Personnellement, je recommande de s'habituer à utiliser strictement les apostrophes partout, car ainsi vous ne devez pas vous souvenir de ce que vous enfermez.

Comportement des guillemets
--------------------------

Attention ! Ne supprimez pas complètement les guillemets ! Ils présentent des avantages particuliers qui peuvent être utiles pour les travaux avancés en PHP. Cependant, les débutants les considèrent comme des erreurs et ne les comprennent pas.

```php
$x = 10; // définir une variable
echo "Hodnota proměnné je: $x, přesně."; // et une liste
```

Entre les guillemets, vous pouvez également énumérer les valeurs des variables, ou le signe dollar fait que tout ce qui le suit est une variable. Donc si vous ne voulez pas sortir la valeur d'une variable, mais le signe du dollar, vous devez échapper.

```php
$price = 25; // prix en dollars
echo "Cena produktu: $price\$"; // imprime "Prix du produit : 25 $".
```

L'avantage des guillemets est discutable dans ce cas et il est peut-être préférable d'utiliser des apostrophes et de lier simplement les chaînes de caractères.

```php
$price = 25;
echo 'Prix du produit :' . $price . '$'; // imprime la même chose que l'exemple précédent
```

> **Note:** Le prix en dollars est correctement écrit dans le format `$25`, ce qui causerait encore plus de confusion, car la notation `$$price` appelle en fait quelque chose appelé une `variable variable` (dans le nom de la variable on appelle le nom de la variable à appeler - juste une confusion).

**TIP:** Vous pourriez être intéressé par <a href="/promenna-variable">variable variable</a>.

Désignation de la chaîne
--------------------------

En général, tout ce qui est entre guillemets ou apostrophes est traité comme une chaîne de caractères. Ainsi :

```php
$x = 5;   // c'est autre chose,
$x = "5"; // que ceci.
```

Dans le premier cas, le **nombre** 5 est stocké dans la variable **$x**, dans le second cas, la **chaîne** "5" est stockée dans la même variable. Heureusement, cela n'a pas d'importance en PHP, et vous pouvez travailler avec l'une ou l'autre variante (presque) de la même manière, car PHP peut automatiquement **transformer** les variables en fonction de leur contenu. Cependant, il est généralement recommandé de ne pas écrire les nombres entre guillemets, notamment pour les opérations de calcul où des erreurs d'arrondi peuvent alors se produire.

Parfois, je peux vouloir stocker la sortie d'une fonction dans une variable :

```php
$pi = 3.14159;
$zaokrouhleni = round($pi); // c'est correct
$zaokrouhleni = "round($pi)"; // c'est "faux" (la question est de savoir à quel résultat je m'attends).
```

L'erreur dans le second cas se trouve juste dans les guillemets, lorsque la sortie de la fonction **round()** n'est pas stockée dans la variable **$round**, mais dans la chaîne appelant cette fonction.
> **TIP:** La valeur `$pi` ne doit pas être saisie directement dans le script comme ceci et nous pouvons utiliser la fonction `pi()`, qui est plus précise pour des calculs plus complexes.

Signification particulière de l'évasion
--------------------------

> Les exemples suivants ne fonctionnent qu'entre guillemets. Si vous les placez entre apostrophes, ils se comporteront comme des caractères normaux sans signification spéciale (sauf ``'`` qui échappe à l'apostrophe).
L'échappement est utilisé lorsque je veux afficher un caractère spécial entre guillemets ou une apostrophe qui pourrait être interprété comme une expression linguistique et donc traité, même si le programmeur ne l'a pas prévu. Nous avons déjà montré un exemple ; cette section décrit les exceptions comportementales possibles.

En effet, il arrive que l'échappement lui-même ait une signification particulière. Exemple :

```php
echo "Texte long, divisé en deux lignes.";
```

L'exemple précédent s'imprime :

```php
Dlouhý text, rozdělený na
dva řádky.
```

Ainsi, si nous voulons écrire un slash, nous devons également l'échapper (l'échappement du caractère `n` n'est pas nécessaire dans ce cas, car il serait compris comme un wrap à nouveau, ou dans ce cas nous ne devons pas échapper du tout) :

```php
echo "Texte long, divisé en deux lignes.";
```

Il existe d'autres caractères spéciaux comme celui-ci, par exemple `\t` qui fait une tabulation. La liste complète se trouve dans la documentation officielle.

Utilisation réelle des caractères spéciaux dans l'échappement
-----------------------------------------------

Pour commencer, je tiens à souligner que presque tout peut être fait en PHP de multiples façons, et les exemples donnés ici ne servent qu'à illustrer la façon dont le problème peut être abordé.

Par exemple, si nous voulons analyser un texte ligne par ligne, nous pouvons utiliser la fonction <a href="/explode">explode</a>.

```php
$parser = explode("\n", $retezec); // analyse le texte ligne par ligne
```

Dans ce cas, l'utilisation du caractère spécial ``n`'' a du sens, car nous pouvons très efficacement dire que nous voulons analyser par les sauts de ligne.

```php
$parser = explode('
', $retezec);
```

> **Avertissement:** Sur les systèmes Unix et Windows, les caractères utilisés pour marquer la fin des lignes sont confondus. Par exemple, Windows utilise `CRLF` (une paire de caractères `\r\n`), alors que Linux utilise seulement `LF` (un seul caractère `\n`). Lors de l'analyse syntaxique par ligne, il faut garder cela à l'esprit. En général, le problème est résolu en normalisant les caractères en `LF` seulement.

Résumé
-------

Si vous le pouvez, utilisez des **apostrophes** partout.

Il est bon de connaître l'usage des guillemets et de ne les utiliser que lorsque cela est nécessaire (ou généralement bon). Si vous produisez un texte susceptible de contenir des guillemets, mettez-le entre apostrophes (qui se comportent alors de manière plus prévisible). Personnellement, j'utilise les guillemets pour exprimer divers caractères spéciaux qui sont inutilement complexes à saisir dans des apostrophes et qui nécessiteraient un échappement complexe.
