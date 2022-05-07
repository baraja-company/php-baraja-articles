Comment écrire votre premier script PHP
=======================================

> id: '2d6986dc-f24b-4ae5-b24c-d5bb9bf94512'
> slug:
> 	cs: prvni-script
> 	fr: comment-ecrire-votre-premier-script-php
> 
> perex: Jak napsat první PHP script? Úvod do PHP pro začátečníky.
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: a5fab0e5edf99323140e497a6fe52322

Avant d'écrire notre premier script PHP, nous devons d'abord expliquer théoriquement comment charger une page à l'aide de PHP.

1. Tout d'abord, l'utilisateur appelle une URL spécifique dans son navigateur web, par exemple `https://baraja.cz`.
2. Ensuite, le navigateur Web construit une "requête", qui est une demande spéciale adressée au serveur Web et envoyée à l'Internet. Il contient des informations sur la page demandée, l'identification et les paramètres de base du navigateur, des informations sur les cookies, etc.
3. Cette `requête` voyage à travers l'Internet jusqu'à un serveur web (le plus souvent `Apache`), qui lit la requête et commence à compiler une réponse.
4. Puisque l'URL appelée est un script PHP et que la demande concerne un fichier nommé `index.php`, `Apache` lit le fichier `index.php` depuis le répertoire racine sur le disque et le transmet à l'interpréteur `PHP`, qui est un programme capable de traiter le code PHP lui-même et de construire un code `HTML` basé sur celui-ci, qui est renvoyé à l'utilisateur.
5. Une fois le code HTML compilé, la réponse est renvoyée à l'utilisateur (c'est ce qu'on appelle une "réponse") et le navigateur Web rend la page de manière normale, comme s'il s'agissait de pur HTML.

> Notez que le navigateur Web n'apprend rien sur le contenu du script PHP, mais traite uniquement le HTML généré, de sorte que vos scripts et le contenu du serveur restent sécurisés.

Créons le premier script
----------------------------

> L'écriture de votre premier script suppose que vous disposez d'un serveur Web sur votre ordinateur. Pour Windows, <a href="https://www.apachefriends.org/index.html">XAMPP</a> est la meilleure solution (téléchargez la version 7.0 ou ultérieure de PHP), et <a href="https://www.apachefriends.org/index.html">XAMPP</a> fonctionne exactement de la même manière sur Mac que sur Windows. Pour Linux, je recommande <a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">le serveur LAMP</a> (ce site fonctionne également sur le serveur Lamp).

Le nom du fichier script PHP doit se terminer par l'extension `.php` afin que le serveur web sache que nous voulons le traiter selon les règles PHP. Créons donc un fichier `index.php`, par exemple, qui contiendra le code de la page principale de notre site web.

Ouverture du fichier
--------------------------

Ouvrez ce fichier dans un éditeur de texte approprié pour écrire le code source.

Sous Windows, par exemple, <a href="https://www.sublimetext.com">Sublime Text</a> est un bon point de départ, car il colore joliment la syntaxe (règles du langage) et rend le code plus facile à lire. Plus tard, je recommande d'acheter <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, qui est très utilisé dans les entreprises et offre la possibilité de programmer plusieurs personnes.

Écrire la structure de base d'une page HTML
--------------------------

Vous connaissez probablement déjà la structure de base d'une page HTML :

```html
<!DOCTYPE HTML>
<html>
    <head>
    <title>Můj první PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>

    </body>
</html>
```

Tout le code HTML sera traité de manière normale et sera d'une grande aide pour la conception du site. PHP utilise dans une large mesure les principes du HTML et du CSS.

Séparation du script PHP du code HTML
--------------------------

PHP est principalement un langage de modélisation qui génère du contenu personnalisé aux endroits appropriés du code. Pour pouvoir distinguer clairement ce qui est HTML de ce qui est PHP, nous devons utiliser une balise de séparation.

Actuellement, il est préférable d'utiliser la notation avec `<?php` et `?>`.

```php
// voici le code PHP
?>
```

> Nous utilisons le terminateur `?>` si nous voulons utiliser un autre code HTML. S'il n'y a plus de code HTML à la fin du script PHP, il est préférable de ne pas inclure la balise `?>`, afin qu'il n'y ait pas d'espaces blancs et de sauts de ligne inutiles en fin de page que l'éditeur de texte puisse insérer.

Dans le passé, la balise `<?` a été beaucoup utilisée à la place de `<?php`, mais elle n'est pas toujours prise en charge.

Les balises enveloppantes peuvent être placées n'importe où dans le code HTML, par exemple dans le corps de la page :

```html
<!DOCTYPE HTML>
<html>
    <head>
    <title>Můj první PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>
    <?php
        // tady bude PHP kód
    ?>
    </body>
</html>
```

Constructions de base
--------------------------

Parmi les éléments de base, on trouve :

- <a href="/echo">Lister le contenu dans le code source</a>
- <a href="/variable">Variables</a>
- <a href="/if">Conditions</a>

Dans cette section, nous allons faire la démonstration d'un simple listage du contenu vers le code source en utilisant une variable.

Le principe de la construction du code source
--------------------------------

Toutes les constructions (expressions du langage), les déclarations et les fonctions sont séparées par des points-virgules afin d'indiquer sans ambiguïté les points de départ et d'arrivée de la construction en cours.

Un point-virgule est généralement suivi d'un saut de ligne.

Symboliquement écrit :

```php
příkaz;
další příkaz;
proměnná x = její hodnota;

vypsat proměnnou x;

uložit do souboru;
```

Extraire vers le code source
--------------------------

La construction <a href="/echo">echo</a> est utilisée pour énumérer le contenu.

Il est très facile à utiliser :

```php
echo 'Bonjour, le monde !';
```

Il imprime ensuite le texte "Hello world !" dans le code HTML. Essayez l'échantillon.

> Toutes les autres démos ne contiendront que l'intérieur du code PHP. Le code HTML qui l'entoure est libre pour que vous puissiez vous faire votre propre idée (par exemple, utilisez l'exemple au début de cet article).

Variables
--------------------------

Les variables sont des emplacements de mémoire virutale qui stockent des données et sont utilisées pour les déplacer. Le nom d'une variable commence toujours par un `dollar`, suivi du `nom` lui-même, puis de sa `valeur`.

> J'ai résumé une description détaillée du fonctionnement des variables dans <a href="/variable">un article séparé sur les variables</a>.

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo;
echo '<br>';
echo $jmenoAutora;
```

> Le nom de la variable doit exprimer ce que la variable contient réellement pour rendre le code plus clair. Notez également l'insertion de la balise HTML `<br>` pour indenter le texte. Vous devriez déjà connaître cette balise dans le langage HTML.

Ce qui est sorti dans la construction `echo` est appelé une chaîne (une séquence de caractères). Les chaînes individuelles peuvent être concaténées avec un point (`.`) pour réduire la sortie à une seule ligne :

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo . '<br>' . $jmenoAutora;
```

> Après avoir relié les cordes avec un point, l'ensemble sera vu comme une seule grande corde.

Opérations variables
--------------------------

Entre les variables, toutes les opérations <a href="/mathématiques">mathématiques de base</a> fonctionnent de manière parfaitement intuitive, comme prévu.

Définissons 2 variables et mettons-y des chiffres :

```php
$x = 5; // définit la variable x avec la valeur 5
$y = 3; // définit une variable y avec la valeur 3

echo $x + $y; // ajoute les variables et imprime 8
```

> Notez que le signe égal (`=`) n'est pas utilisé pour effectuer une opération mathématique, vous ne pouvez donc pas écrire d'équations, par exemple. PHP se comporte comme une calculatrice à cet égard.

Si nous ne voulons pas utiliser de variables, nous pouvons effectuer les opérations directement. Ainsi, l'emplacement des opérations n'a pas d'importance et elles peuvent être évaluées n'importe où.

```php
echo 5 + 3; // imprime 8
```

On peut aussi additionner les variables et stocker le résultat dans une autre variable :

```php
$x = 5;
$y = 3;
$z = $x + $y; // la variable $z contient 8

echo $z; // imprime 8
```

Dans la prochaine partie, nous apprendrons les bases complètes de <a href="/principes-de-variables">la définition et l'utilisation des variables</a>.
