Indenter le code en utilisant des espaces et des tabulations
============================================================

> id: '116f19ed-3753-498d-bb9e-e0f93b88c347'
> slug:
> 	cs: mezery-a-tabulatory
> 	fr: indenter-le-code-en-utilisant-des-espaces-et-des-tabulations
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c3af36d7df67cf36b940c9702cb71549

Pour que le code soit facile à lire pour les autres programmeurs et qu'il reste élégant, nous devons apprendre à le formater de manière uniforme. Cet article traite de l'utilisation des espaces et des tabulations.

Les espaces ou les tabulations sont-ils meilleurs pour l'indentation du code ? Il s'agit souvent d'un sujet de débat sans fin. Si vous cherchez une réponse rapide et sans ambiguïté, la plupart des bons programmeurs préfèrent utiliser les tabulations, mais décomposons le tout gentiment.

Espaces
----------------------

Chaque programmeur et éditeur utilise un nombre différent d'espaces pour l'indentation (mais le plus souvent 4), ce qui conduit à un code incohérent qui peut être plus difficile à lire lorsqu'on lit le code de quelqu'un d'autre. En outre, davantage de caractères sont nécessaires pour l'indentation (ce qui augmente la taille des données).

Cependant, les espaces présentent un avantage lors du rendu du code dans un navigateur web (où l'entité HTML `&nbsp;` est utilisée pour l'indentation), il s'agit donc d'un format relativement facile à porter qui ne gagne un avantage qu'en tant que méthode de rendu stable et fiable (4 espaces apparaîtront toujours comme 4 espaces).

Tabulateurs
----------------------

Elles ont la largeur que le programmeur définit dans l'éditeur (si l'éditeur peut le faire), donc si vous aimez une indentation particulière, pas de problème - nous pouvons tous regarder le même code avec des largeurs de tabulation différentes. En même temps, c'est un caractère très économique qui n'a pas besoin d'être répété aussi souvent que de simples espaces.

Lors du rendu d'un code avec tabulation dans une page HTML, il est d'usage de remplacer les tabulations par des espaces fixes afin de garantir un affichage correct dans tous les navigateurs :

```php
$code = <?php
    $a = 5+3 ;
    $b = 4 ;
    if ($a > $b) {
        echo $a . " > " . $b ;
    } else {
        echo $b . " <= " . $a ;
    }
?>';

echo str_replace("\t", '&nbsp ;', $code);
```
