Immutabilité des objets - un concept clé de la conception
=========================================================

> id: '057467db-4e3b-4e18-9ea5-dfb25feb3800'
> slug:
> 	cs: immutabilita
> 	fr: immutabilite-des-objets-un-concept-cle-de-la-conception
> 
> publicationDate: '2022-07-24 15:00:00'
> mainCategoryId: ae4c1c70-11b3-433e-b1d0-e590155bb8b9
> sourceContentHash: '88c26f35883c860426b4708f0be8761f'

L'immuabilité est l'un des concepts de conception les plus importants pour construire des applications stables. Le principe de base veut qu'une fois qu'un état est écrit, il ne peut être lu que plus tard sans possibilité de le modifier. Si nous devons changer l'état, nous devons créer une nouvelle instance et remplacer l'objet entier par un autre.

Les types de données peuvent donc être très grossièrement divisés en deux grandes catégories :

- **Mutable** (état mutable au sein d'une seule instance)
- **Immutable** (état interne immuable)

Les objets mutables peuvent être modifiés en interne. C'est-à-dire qu'ils fournissent des opérations qui, lorsqu'elles sont appelées dans différentes combinaisons, nous font obtenir des résultats différents. L'immuabilité tente d'empêcher ce comportement.

Définition
--------

> Une classe est **immuable** précisément si les données de l'instance ne peuvent être modifiées d'aucune manière après la création de l'instance.

Ainsi, toutes les données sont fixées dans le constructeur. Tous les types de données scalaires sont également automatiquement immuables.

Un avantage majeur
--------------

La conception d'applications avec des états immuables offre un avantage fondamental en matière de sécurité d'exécution des opérations. Si nous savons qu'une fois écrites, les données ne peuvent pas être modifiées (mutées) par la suite, nous pouvons, par exemple, déboguer de manière très fiable ou diviser l'application en sous-fonctions sans risquer d'oublier l'un des états intermédiaires.

L'idée d'immuabilité s'oppose généralement au principe de stockage des états dans les propriétés des objets/entités, et décrit plutôt une approche fonctionnelle où les données ne font que "circuler" dans l'application, comme le fait javascript par exemple.

Du point de vue des performances, on peut dire automatiquement des objets immuables qu'ils peuvent être mis en cache indéfiniment, car ils ne seront jamais périmés.

Un exemple concret en PHP
--------------------

De loin l'utilisation la plus courante des objets immuables en PHP est l'objet `DateTimeImmutable`, qui une fois créé ne peut être appelé que par des méthodes de formatage. Si nous modifions les paramètres internes, la méthode renverra une nouvelle instance. Cette fonctionnalité est cruciale lorsqu'on utilise un ORM qui utilise le modèle dit d'identité - elle nous permet, par exemple, de garantir que lorsque nous lisons la date de création d'une commande, elle sera la même partout dans l'application et que l'intégrité référentielle ne sera pas corrompue.

Un exemple concret d'un objet mutable :

```php
$date = new DateTime('2021-05-14');
$tomorrow = $date->modify('+1 jour');
echo $date->format('Y-m-d'); // 2021-05-15
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

La même date a été imprimée car la méthode `modify()` n'a fait que modifier l'état interne de l'objet `DateTime` et a retourné la même instance. Ainsi, il n'y avait pas de **mutation de l'état interne**, qui est un comportement de base de la programmation orientée objet. La mise à jour de la variable a également modifié la variable d'origine.

Et maintenant, un exemple d'objet immuable :

```php
$date = new DateTimeImmutable('2021-05-14');
$tomorrow = $date->modify('+1 jour');
echo $date->format('Y-m-d'); // 2021-05-14
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

L'objet `DateTimeImmutable` est immuable, ce qui signifie que son état interne ne change jamais. Une nouvelle instance modifiée (également immuable) est stockée dans la variable après l'appel de la méthode `modify()`. Si nous ne stockons pas la nouvelle valeur dans la variable, elle ne sera pas utilisable plus tard.

La valeur originale n'est jamais touchée et reste stockée en toute sécurité.

Quand une classe doit-elle être immuable ?
---------------------------

À moins que vous n'ayez une très bonne raison de la rendre mutable, écrivez toujours une classe ou une fonction comme étant immuable. Cela simplifiera votre conception à l'avenir.

Les classes mutables doivent changer le moins possible. Je recommande toujours de documenter le comportement de l'immuabilité.

Le seul inconvénient de l'immuabilité est peut-être qu'une nouvelle instance doit être créée à chaque changement d'attribut, ce qui a un impact mineur sur les performances. Si votre application (comme la plupart des applications) a tendance à afficher des données et à les modifier moins fréquemment, cet inconvénient est plutôt insignifiant au regard des performances des ordinateurs actuels.

Quels types de données doivent être immuables ?
------------------------------------

- Tous les identifiants et codes uniques
- La plupart des sessions de base de données ManyToOne et OneToOne
- Dates, heures, valeurs de calendrier
- Cyclage dans des tableaux où l'on veut effectuer la même opération sur chaque élément.
- Intervalles, paires, triples, ...
- Figures géométriques, points, lignes, coordonnées GPS, adresses physiques, ...
- Journaux et enregistrements historiques
- Informations sur les commandes traitées et la plupart des données financières
- Méta-données sur l'entité liée

Ce qui ne devrait pas être immuable :

- Grands objets avec beaucoup de propriétés
- La plupart des sorties tabulaires d'une base de données, comme les entités Doctrine
- Construire progressivement des objets à partir de pièces plus petites
