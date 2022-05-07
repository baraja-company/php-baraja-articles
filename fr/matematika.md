Les mathématiques en PHP
========================

> id: '69a432ee-7635-46e2-bb21-8492cb62c4e6'
> slug:
> 	cs: matematika
> 	fr: les-mathematiques-en-php
> 
> perex: 'Matematika a matematické funkce v PHP. Definice čísel, operátorů a vhodné typy pro výpočty a ukládání čísel.'
> publicationDate: '2020-02-16 18:24:10'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '1070f11c80eff1faf3388e47721b01a2'

Comme dans d'autres langages, les nombres en PHP sont représentés dans le système binaire (le système des uns et des zéros), donc certaines opérations mathématiques peuvent produire des résultats légèrement différents de ceux auxquels on pourrait s'attendre. Cette page donne un aperçu du comportement des calculs dans différents contextes. Au moins une connaissance de base des **techniques numériques** et des **systèmes numériques** est nécessaire pour la compréhension.

Représentation des nombres en mémoire
---------------------------

La façon dont les nombres sont représentés est caractérisée par le type de données, par exemple integer est converti directement à partir du code source de décimal à binaire. Nous n'avons pas à nous soucier de la conversion des chiffres, tout se fait automatiquement.

La conséquence de cette conversion est que la valeur maximale d'un nombre entier est déterminée par le nombre de bits disponibles en mémoire. En PHP 32 bits, l'intervalle est de `-2 147 483 648` à `-2 147 483 647` (~ ± 2 milliards), en PHP 64 bits l'intervalle est de `-9 223 372 036 854 775 808` à `-9 223 372 036 854 775 807` (~ ± 9 quintillions). La valeur maximale peut toujours être obtenue en appelant la constante `PHP_INT_MAX`.

Les nombres décimaux sont stockés dans le type de données **float**, dont la précision est limitée. Ils sont donc stockés en interne comme une paire de nombres soumis à l'opération mathématique `mantisse * (2^exposant)`. La conséquence pratique est que nous sommes capables de stocker un nombre relativement important (de l'ordre de milliards) dans un petit nombre de bits, mais pas de manière très précise.

La conséquence de l'utilisation du type de données **float** est que le résultat du calcul de `1 - 0.9` n'est pas exactement `0.1`.

Exemple tiré de <a href="https://diskuse.jakpsatweb.cz/?action=vthread&forum=9&topic=97912">forums web</a> :

```php
$x = 0.3;
$y = 0.4;
echo 0.7 - $x - $y; // imprime -5.5511151231258E-17
```

Si nous ajustons légèrement les chiffres :

```php
$x = 6.5;
$y = 7.5;
echo 14.0 - $x - $y; // imprime 0
```

En général, cette question est <a href="https://vtm.zive.cz/clanky/proc-pocitacum-delaji-problemy-desetinna-cisla/sc-870-a-185672/default.aspx">discutée dans un article séparé sur VTM.e15.cz : Pourquoi les ordinateurs ont des problèmes avec les nombres décimaux</a>, ou en général sur <a href="https://cs.wikipedia.org/wiki/Pohyblivá_řádová_čárka">le point flottant sur Wikipedia</a>.

> En général, il est bon d'arrondir le résultat après le calcul, ou d'éviter complètement les décimales. Par exemple, enregistrez le prix non pas en couronnes (comme 14,90) mais en centimes (comme 1490) et travaillez ensuite avec précision. L'arrondi est abordé dans la section suivante de cet article.

Opérations mathématiques
-------------------

```php
$a = 5;
$b = 3;

echo $a + $b;
```

Des conventions mathématiques communes peuvent être utilisées pour les opérations, qui respectent la préséance des opérateurs selon les règles des mathématiques (la multiplication prime sur l'addition, etc.). Les valeurs peuvent être écrites directement ou lues via des variables.

> Cela peut ne pas être évident au premier abord, mais dans l'exemple mathématique, le signe égal (`=`) n'est pas écrit. Il s'ensuit que seules les `opérations binaires` simples peuvent être résolues, qui fonctionnent toujours avec seulement `deux éléments` (ne comptez pas les équations, vous devez utiliser une bibliothèque spécialisée pour cela).

Aperçu des opérateurs de base
----------------------------

> Dans la colonne `resultat`, je liste ce qui est imprimé en supposant :

```php
$a = 5;
$b = 3;
$c = 10;
```

| Opération | Marque | Marque en mots | Exemple de notation | Résultat
|-------------------|-----------|---------------|-----------------------|----------
| Addition | `+` | Plus | `echo $a + $b;` | 8
| Soustraction | `-` | Moins | `echo $a - $b;` | 2
| Multiplication | `*` | Astérisque | `echo $a * $b;` | 15
| division | `/` | barre oblique | `echo $a / $b;` | 1.6666666666667
| parenthèses | `( )| parenthèses | `echo $a + ($b * $c);`| 35
| Concaténation de chaînes de caractères | `.` | Période | `echo $a . $b . $c;` | 5310
| Reste après division | `%` | Pourcentage | `echo $a % $b;` | 2
| Ajouter un | `++` | deux plus | `echo $a++;` | 6
| Soustraire un | `--` | deux moins | `echo $a--;` | 4
| Power | `**` | deux astérisques | `echo $a ** $b;` | 125

> L'opérateur puissance (double astérisque) n'est disponible qu'à partir de PHP 7.1. Dans les autres versions de PHP, vous devez utiliser la fonction universelle `pow($a, $b)`.

Aperçu des fonctions mathématiques de base
---------------------------------------

Si vous devez résoudre une tâche informatique courante, il est fort probable qu'il existe déjà une fonction directement en PHP. Voici un aperçu annoté de ces fonctions.

Arrondir
--------------

L'arrondi normal se fait avec la fonction <a href="/fonction-round">round()</a>, elle a 3 paramètres :

- Nombre arrondi
- Facultatif : Précision (jusqu'à combien de décimales)
- Facultatif : Valeur de division par deux (à partir de quelle valeur arrondir)

```php
round(5);               // 5
round(5.1);             // 5
round(5.4);             // 5
round(5.5);             // 6
round(5.8);             // 6
round(5483.47621, 2);   // 5483.47
round(5483.47621, -2);  // 5500
```

Il existe également une <a href="function-floor">floor()</a> fonction pour arrondir à la baisse et une <a href="/function-ceil">ceil()</a> fonction pour arrondir à la hausse.

**Attention aux types de données!**
>
> Toutes les fonctions d'arrondi renvoient le résultat sous forme de `float`, même si le résultat est un entier.
>
> > Si vous voulez traiter le résultat comme un nombre entier, il est important de surtyper le résultat. Par exemple : `echo (int) round(3.14);`

PHP travaille avec différents types de données, par exemple avec **float** il peut facilement arriver que le résultat de `round()` ne soit pas un nombre avec une expansion décimale finie. Pour la sortie, je recommande d'utiliser la fonction `number_format()`, dont je parle ci-dessous.

Fonctions goniométriques
--------------------

Ils sont utilisés pour de nombreux calculs différents et sont basés sur le cercle des unités. Ils sont utilisés, par exemple, pour tracer des cercles, des ellipses, des déplacements, etc.

- <a href="/fonction-sin">sin()</a>
- <a href="/fonction-cos">cos()</a>
- <a href="/fonction-tan">tan()</a>

Fonctions cyclométriques
--------------------

Calculez l'angle entre `x` et `y`. Conversions des coordonnées cartésiennes et polaires.

- <a href="/fonction-asin">asin()</a>
- <a href="/fonction-acos">acos()</a>
- <a href="/fonction-atan">atan()</a>

Fonctions hyperboliques
-------------------

Basé sur l'hyperbole unitaire. Leurs définitions peuvent être facilement décrites à l'aide de simulations :

- Ellipse, cercle, cercle de rayon 1
- Hyperbole, Hyperbole équiaxiale, Hyperbole unitaire

Ils sont utilisés, par exemple, pour la génération de terrains et la simulation physique.

- <a href="/fonction-sinh">sinh()</a>
- <a href="/fonction-cosh">cosh()</a> (chainline)
- <a href="/fonction-tanh">tanh()</a>

Fonctions hyperbolométriques
------------------------

Basé sur l'hyperbole unitaire.

- <a href="/fonction-asinh">asinh()</a>
- <a href="/fonction-acosh">acosh()</a>
- <a href="/fonction-atanh">atanh()</a>

Exemples d'utilisation des fonctions goniométriques
---------------------------------------

```php
echo sin(30);            // -0.98803162409286
echo sin(deg2rad(30));   // 0.5
echo cos(deg2rad(123));  // -0.54463903501503
echo 1/tan(deg2rad(45)); // 1 (onen cotangente)
echo sin(deg2rad(500));  // 0.64278760968654
echo atan2(deg2rad(50), deg2rad(23)); // 1.1396575860761
```

Calculatrice - traitement des expressions mathématiques
--------------------------------------------

Il peut arriver que nous devions traiter une expression mathématique comme une chaîne utilisateur, par exemple `5+2^(1+3/2)`.

Il n'y a pas de moyen simple de traiter de telles entrées en PHP, mais j'ai programmé une série de bibliothèques qui traitent ce problème.

Pour plus de détails, voir l'article séparé <a href="/pokrocila-calculatrice">Calculatrice en PHP : traiter une expression mathématique sous forme de chaîne de caractères</a>.

Opérations avec de grands nombres et précision des calculs
------------------------------------------

Les grands nombres et les décimales sont stockés comme des flottants en PHP, et ils sont arrondis à environ 15 décimales, ce qui peut parfois être gênant. C'est pourquoi la bibliothèque **<a href="https://www.php.net/manual/en/book.bc.php">BCMath Arbitrary Precision Mathematics a été créée</a>**.

Les nombres sont représentés comme des **chaînes** dans cette bibliothèque, ils ne sont donc pas limités par l'imprécision de **float**, mais les opérations effectuées sont plus lentes de plusieurs ordres de grandeur (mais toujours plus rapides que si nous programmions nous-mêmes une telle fonction).

Par exemple, si nous voulons obtenir une racine carrée de 2 à 3 décimales, il suffit d'appeler :

```php
echo bcsqrt('2', 3); // 1.414
```
