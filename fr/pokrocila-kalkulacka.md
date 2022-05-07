Calculatrice en PHP : traitement d'une expression mathématique sous forme de chaîne de caractères
=================================================================================================

> id: e6798758-031d-4e7c-b24e-1f77cf61558d
> slug:
> 	cs: pokrocila-kalkulacka
> 	fr: calculatrice-en-php-traitement-d-une-expression-mathematique-sous-forme-de-chaine-de-caracteres
> 
> publicationDate: '2020-02-16 17:07:38'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '63f5336bf2bbabe5312121b57e1ce34e'

Imaginez que vous soyez confronté au traitement d'un exemple mathématique simple qu'un utilisateur saisit sous forme de chaîne de texte dans un champ de recherche. En général, l'utilisateur souhaite effectuer une opération numérique simple avec des nombres. Cet article décrit le processus de réflexion et les instructions spécifiques sur la façon de le faire.

Mise en œuvre naïve
-------------------

Pendant longtemps, je me suis demandé si une simple expression mathématique pouvait être traitée par une astuce pour rendre le code le plus court possible... et après de nombreuses années, j'ai effectivement la solution.

Considérez la solution donnée **uniquement à titre d'exemple**, car elle est **extrêmement dangereuse** et un utilisateur malhonnête peut facilement souligner la chaîne, ce qui par exemple supprimera toute l'application ou volera la base de données !

```php
// Requête de l'utilisateur
$query = '5 + 3 * 2';

// Traitement de l'expression en tant que code PHP régulier
eval('$result = @(' . $query . ') ;');

// Listing de la variable avec l'expression solution
echo $result; // imprime 11
```

L'astuce est que la fonction <a href="/function-eval">eval()</a> exécute la chaîne de caractères comme si elle se trouvait dans le contexte du code PHP. C'est fou, mais ça marche. Le wrapper supprime les messages d'erreur.

Traitement d'entrées plus complexes
--------------------------

Outre le fait que **traiter des expressions via eval() est extrêmement dangereux**, il ne fournit pas non plus une syntaxe suffisamment éloquente pour convenir à tout le monde. Si l'utilisateur commet ne serait-ce qu'une seule violation de la syntaxe, l'expression entière ne pourra pas être traitée.

Par conséquent, la solution consiste à **comprendre** et à corriger la requête de l'utilisateur en fonction de l'aspect formel d'abord (ce qu'on appelle la normalisation à la forme canonique), puis à la transmettre et à la traiter ensuite.

J'ai programmé [QueryNormalizer] (https://github.com/mathematicator-core/engine/blob/master/src/QueryNormalizer.php) exactement pour cette tâche dans le passé.

Le traitement lui-même est une tâche très difficile, car vous devez comprendre correctement les différents contextes. Par exemple, que les parenthèses indiquent des blocs imbriqués et doivent être évalués de manière récursive. Par exemple, l'expression `5+2^(1+3/2)` ne peut pas être résolue de manière directe car il faut d'abord résoudre la fraction, l'ajouter au nombre entre parenthèses, puis la résoudre pour la puissance entière, et enfin l'ajouter au niveau de la base.

Pour pouvoir répondre à cette exigence, l'expression ne peut plus être traitée comme une chaîne ordinaire et nous devons **passer au niveau d'abstraction**. Par essence, les mathématiques sont une sorte de langage qui décrit les relations entre les opérations et les nombres, car nous devons traiter les priorités des opérateurs, les différentes significations, les contextes, la récursion et même les types de données. C'est là que le **processus de tokenisation des requêtes** entre en jeu.

> Je travaille sur le problème de la tokenisation mathématique depuis 2015 et j'ai écrit plusieurs parsers différents depuis.

Le meilleur d'entre eux, qui équipe actuellement le nouveau Mathematicator, est [disponible en code source libre sur GitHub] (https://github.com/mathematicator-core/tokenizer).

Le but de la tokenisation est de **parser une chaîne de caractères**, de la diviser en groupes de chaînes plus petites de types connus, puis de convertir ces dernières en objets (types de données). Le tableau d'objets converti est ensuite converti par **une logique intelligente en un arbre binaire** qui peut décrire les dépendances et la récursion. Il s'agit d'un processus très exigeant car il existe des centaines de scénarios possibles et les utilisateurs peuvent être très créatifs dans leurs requêtes.

Le principal avantage du tableau de jetons est qu'il peut être très facilement transmis à la couche suivante, qui par exemple [effectue le calcul proprement dit] (https://github.com/mathematicator-core/calculator), ou [redessine l'arbre en LaTeX] (https://github.com/mathematicator-core/tokenizer/blob/master/src/TokensToLatex.php).

L'utilisation peut être élégante comme ceci :

```php
$tokenizer = new Tokenizer(/* quelques dépendances */);

// Convertir la formule mathématique en un tableau de jetons :
$tokens = $tokenizer->tokenize('(5+3)*(2/(7+3))');

// Vous pouvez maintenant convertir les jetons en un format plus utile :
$objectTokens = $tokenizer->tokensToObject($tokens);

dump($objectTokens); // Retourne les jetons typés avec les méta-données.

// Rendu à LaTeX
echo $tokenizer->tokensToLatex($objectTokens);

// Rendu à l'arbre de débogage (extrêmement rapide) :
echo $tokenizer->renderTokensTree($objectTokens);
```

Voir les procédures
-----------------

Un grand nombre d'utilisateurs apprécieront que **lorsque le programme calcule, il affiche la procédure** pour montrer comment il a procédé. En fait, cela est également utile pour le programmeur, car il peut au moins trouver facilement où se trouve une erreur de calcul et corriger l'algorithme en conséquence. Lorsque vous combinez tout cela avec l'apprentissage automatique basé sur des tests automatisés, vous obtenez quelque chose d'étonnant.

Regardez comment `QueryNormalizer` a pu comprendre votre requête, passer les données au tokenizer, il a rendu la requête en LaTeX en fonction de celle-ci et a ensuite passé l'arbre d'objets au calculateur qui a retourné le résultat global.

Příklad: [5+2^(1+3/2)](https://mathematicator.com/search/5%2B2%5E%281%2B3/2%29).

La représentation de la procédure est implémentée par le calculateur qui parcourt l'arbre d'entrée et évalue une règle à la fois en fonction des tokens et des règles qu'il contient. Lorsqu'une règle est évaluée, elle place les informations relatives à l'étape dans un tableau. Parfois, une étape peut s'avérer erronée et nous devons revenir en arrière et prendre un chemin différent dans le calcul, mais il y a beaucoup de magie derrière cela, qui restera cachée pour l'instant et que vous pourrez étudier dans l'implémentation.

Conclusion
-----

La procédure ci-dessus décrit comment traiter de manière élégante les expressions mathématiques dans lesquelles nous avons des nombres, des opérations et des relations avec eux. Cette approche ne permet pas, par exemple, de modifier des expressions ou de résoudre des équations, mais nous y reviendrons la prochaine fois.

*Si vous avez d'autres idées sur la façon de traiter les mathématiques efficacement, je serais heureux de les entendre.*
