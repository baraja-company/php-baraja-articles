Modèles de conception en PHP
============================

> id: c22b3e54-bb66-4196-a998-f4b62b9308b2
> slug:
> 	cs: navrhove-vzory
> 	fr: modeles-de-conception-en-php
> 
> publicationDate: '2020-02-13 10:32:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '9ee32f0a3ce55bad52725f45fcd1c041'

Les modèles de conception sont des façons de penser la programmation.

Ils constituent un recueil de conseils, de pratiques prêtes à l'emploi, de bonnes pratiques et d'éclairages sur le développement. Pour chaque paradigme de programmation et chaque type de tâche, il existe certains modèles de conception qui sont les mieux adaptés.

Avantages - pourquoi utiliser des modèles de conception ?
---------------------------------------

En programmation, la résolution de certains types de problèmes est répétitive, il est donc logique de choisir une méthode pour résoudre ces problèmes et de répéter cette méthode.

Le principal avantage se manifeste surtout lors du développement en équipe, lorsque chacun sait comment l'application sera développée (selon quel modèle de conception) et qu'il suffit de l'appliquer. Cela élimine alors les dizaines d'heures inutiles passées à déboguer un code étrangement écrit et à essayer de comprendre les principes voulus par l'auteur.

MVC - un exemple d'utilisation d'un modèle de conception
--------------------------------------

Mon modèle de conception préféré est le `MVC` (de `Model View Controller`), qui dit que l'application est divisée en 3 couches indépendantes qui s'appellent les unes les autres séquentiellement et se transmettent des données.

Par exemple, lors du rendu d'une page, on pourrait croire qu'il décide d'abord de quel type de page il s'agit (par exemple, un détail de catégorie), donc il appelle `CategoryController` avec la méthode `detail`.

Un exemple concret (je simplifie beaucoup) :

```php
class CategoryController
{
    public CategoryManager $categoryManager;

    public function actionDetail(string $id): void
    {
        $this->template->id = $id;
        $this->template->category = $this->categoryManager->getById($id);
    }
}
```

> **Note:**
>
> Il s'agit seulement d'un exemple de code qui explique le principe du modèle de conception `MVC`.
>
> Dans une implémentation réelle, il faudrait encore trouver comment, par exemple, obtenir une instance de `CategoryManager` et comment la passer à la propriété. Généralement, l'injection de dépendances est utilisée pour ce type de tâche.

Avant de rendre la page pour le détail de la catégorie, le `CategoryController` est d'abord appelé pour recevoir la demande réelle (c'est-à-dire, nous rendons le détail de la catégorie avec un certain ID, qui est obtenu par le routeur, par exemple, dans l'URL), obtenir les données (en interrogeant le `Model` correspondant) et passer les données finales au modèle pour le rendu.

L'énorme avantage de ce principe est que nous pouvons écrire de nombreux modèles (logique d'application) qui sont indépendants de la façon dont les données sont présentées (le modèle), ce qui permet d'obtenir un code réutilisable. En fait, si nous voulons utiliser le `CategoryManager` dans un autre projet, nous passons simplement les données à travers le `Controller` d'une manière spécifique, qui sera rendu selon le modèle défini par le projet lui-même, tandis que la logique de l'application reste la même et personne ne s'en soucie car la couche logicielle a rempli son interface et ses responsabilités convenues.

> **Note pratique:**
>
> Le modèle de conception `MVC` est utilisé par la plupart des frameworks modernes tels que Nette, Symfony, Laravel et autres.
>
> Nous pouvons également rencontrer `MVC` dans le développement d'applications mobiles et d'autres types de logiciels où nous devons obtenir des données et les rendre dans un modèle en fonction du type de page ou de vue.

Modèles de conception importants pour le développement web
---------------------------------------

En général, en programmation, il existe de nombreux modèles de conception qui ne conviennent pas au développement web. Cette liste décrit les motifs les plus importants que j'utilise moi-même et avec lesquels vous devriez être familiarisé.

Une <a href="/category-design-patterns">liste complète de tous les design patterns</a>, des exemples de leur utilisation et des explications détaillées se trouvent sur une page séparée.

- **MVC** - Principe consistant à diviser une application en `Modèle` (logique et données de l'application), `Vue` (vue du modèle et des données) et `Contrôleur` (reliant `Modèle` et `Vue`).
- **Injection de dépendances** - Au lieu de créer des instances de classes, nous définissons des services que nous avons automatiquement injectés. Le code admet toutes les dépendances, les parties individuelles peuvent être remplacées et nous construisons l'application de manière dynamique.
- **Singleton (Singleton)** - Chaque classe n'a qu'une seule instance (je l'utilise personnellement en combinaison avec l'injection de dépendance, où chaque service n'a qu'une seule instance, qui est transmise à travers l'application).
- **Lazy Initialization (Delayed Initialization)** - Nous créons une instance d'un objet lorsqu'il est nécessaire pour la première fois.
- **Adaptateur** - Si nous devons assurer la communication entre deux classes incompatibles, l'adaptateur convertit les données d'un type à l'autre (typiquement, il convertit les types de données natives de PHP en types de données de base de données et inversement).
- **Commande** - Au lieu d'exécuter directement une tâche spécifique (par exemple, l'envoi d'un courriel), nous nous souvenons simplement qu'elle était censée se produire. Un autre ou le même processus reprendra alors la demande et la traitera. Le principal avantage est que nous pouvons traiter l'application de manière paresseuse (et donc très rapide), relancer les actions qui ont échoué et simplement stocker les paramètres de l'appel. En même temps, cela nous permettra de recevoir des demandes de différents clients, via des API, etc. Les actions envoyées peuvent également être mises en file d'attente, classées par ordre de priorité et éventuellement annulées avant leur évaluation.
- **Stratégie** - Encapsule un certain type d'algorithmes ou d'objets à modifier afin qu'ils soient interchangeables pour le client.

Il existe de nombreux autres modèles de conception, mais ce sont les plus importants que vous devez connaître.

Anti-modèle
------------

Certaines techniques de développement de la programmation sont considérées comme des "anti-modèles", c'est-à-dire l'exact opposé d'un modèle de conception. Il s'agit généralement d'une technique qui produit un code étrange qui ne peut pas être facilement débogué, maintenu, et qui se comporte de manière `magique`.

Un exemple typique est l'utilisation de <a href="/global-variable">variables globales</a>.
