Philosophie de base de la programmation orientée objet
======================================================

> id: c38214d9-8c92-4484-9c31-239d716f2545
> slug:
> 	cs: filosofie-oop
> 	fr: philosophie-de-base-de-la-programmation-orientee-objet
> 
> publicationDate: '2020-01-02 22:21:56'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3841046b40a039413bacd045f275c68

La programmation orientée objet est un paradigme, une vision de la manière de programmer. Vous verrez bientôt par vous-même que la POO apporte une simplification assez fondamentale à tous les problèmes et difficultés courants qui sont résolus encore et encore dans la programmation réelle.

L'idée de base
-----------------

L'idée de base de la programmation orientée objet est de diviser une grande application (une tâche complexe) en de nombreuses petites parties que nous pouvons résoudre de manière élégante et indépendante.

Par exemple, si nous programmons un système de réservation de billets d'avion, il s'agit d'un projet très complexe qui résout des milliers de cas. Si nous pouvons décomposer l'ensemble de la logique complexe en plusieurs couches et parties, nous pouvons facilement comprendre l'ensemble du système complexe et programmer les sous-tâches individuelles de manière indépendante.

Avantages pratiques de la POO et de la division du code en objets
------------------------------------------------

En dehors du point de vue académique, il existe de nombreuses raisons pratiques d'utiliser la POO :

- L'application créera une <a href="/encapsulation">encapsulation</a>, ce qui signifie que les données sont toujours traitées dans un contexte local, qui est peu nombreux, de sorte que nous ne devons pas penser à l'ensemble de l'application en même temps.
- Nous pouvons diviser une application en centaines de milliers de petites parties qui peuvent être développées par différentes personnes en parallèle et simplement organiser une interface commune.
- Nous pouvons examiner l'application du point de vue de différentes couches (de manière abstraite pour des modules entiers, ou localement pour des fonctions spécifiques résolvant un algorithme spécifique).
- Lors du débogage de l'application (débogage), nous sommes en mesure d'échelonner l'application, d'omettre ou de remplacer certaines parties.
- Nous pouvons mieux appliquer les **design patterns** et ainsi prévenir les erreurs et les complexités dans le code avant qu'elles ne se produisent.

Personnellement, je ne peux pas imaginer que les équipes composées de plus d'une personne programment différemment.

> **Note:**
>
> L'utilisation d'objets entraîne une augmentation de la mémoire et de la charge du processeur sur l'ordinateur, ce qui réduit un peu les performances de l'application. Dans un environnement réel, cependant, cela n'a pas d'importance, car la reprogrammation de l'application vers les objets entraîne une perte de performances (généralement des unités de pourcentage), mais fait gagner du temps aux programmeurs (généralement des dizaines ou des centaines de pour cent). Le temps humain est toujours beaucoup plus cher (et très limité) que le temps informatique.
>
> N'oubliez pas non plus que la POO simplifie grandement l'ensemble de l'application et permet de réaliser de grandes applications en un temps raisonnable. Un grand nombre d'applications complexes seraient presque impossibles à programmer sans objets.

Relation avec le monde réel
-------------------------

L'objectif fondamental de la POO dans la conception de logiciels est de simuler autant que possible les propriétés, les comportements et les principes du monde réel. Les objets dans la POO représentent des entités réelles. Cette façon de penser nous permet de construire d'énormes systèmes complexes qui peuvent être bien compris, de résoudre des problèmes du monde réel en interne comme ils le seraient sans ordinateur, et dont les principes peuvent être expliqués à des personnes réelles.

Par exemple, si nous mettons en œuvre une application de gestion de contenu, il est logique d'organiser toute la logique interne en plusieurs entités réelles (article, auteur, catégorie) et de construire des sessions non pas en fonction d'identifiants d'enregistrement générés artificiellement (comme c'est le cas dans les bases de données) mais en fonction de relations réelles.

Exemple de mise en œuvre concrète :

```php
class Article
{
    private Author $author;

    /** @var Category[] */
    private array $categories;

    private string $title;
}

class Author
{
    private string $name;
}

class Category
{
    private string $name;
}
```

Comme nous pouvons le voir, la classe `Article` ne contient pas seulement des paramètres techniques tels que l'ID de l'enregistrement avec l'auteur, mais c'est une véritable liaison à l'entité Auteur, qui a de nouveau ses propres propriétés.

Pour une explication de l'implémentation et de la syntaxe spécifiques, voir le <a href="/uvod-do-oop">tutoriel d'introduction</a>.
