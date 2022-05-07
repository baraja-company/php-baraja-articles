Héritabilité et visibilité dans les EPI
=======================================

> id: '71896a55-dcfd-4bb6-9f53-64f22b1514eb'
> slug:
> 	cs: dedicnost-a-viditelnost
> 	fr: heritabilite-et-visibilite-dans-les-epi
> 
> publicationDate: '2020-02-16 22:17:05'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '4584f3dcfdcfdc506d34a37c36fe6dd1'

L'une des propriétés fondamentales de la programmation orientée objet est l' **héritage** et <a href="/encapsulation">encapsulation</a>. Grâce à ces fonctionnalités, vous pourrez facilement construire une logique d'application complexe tout en maintenant une bonne lisibilité de l'implémentation.

Le principe de l'héritage
-------------------

L'héritage exprime que l'implémentation d'une classe est basée sur une autre. Dans la terminologie de la POO, on parle de **descendant** (la classe qui hérite) et de **ancestor** (la classe dont on hérite).

En général, l'héritage fonctionne de la manière suivante : le descendant obtient toutes les caractéristiques de l'ancêtre, soit en les adoptant exactement comme l'ancêtre les avait, soit en les modifiant à sa manière, soit en les remplaçant complètement et en utilisant sa propre implémentation.

L'utilisation de cette approche est très large, et l'héritage est utilisé par un certain nombre de <a href="/design-patterns">design patterns</a>.

Utilisation réelle de l'héritage - présentateurs dans une application
--------------------

L'héritage est bien adapté à la conception de ce que l'on appelle les **présentateurs**, qui sont un type spécial de classe représentant la logique de liaison dans le modèle de conception **MVC**.

Par exemple, prenons un trio de pages "Accueil", "Contact" et "Connexion".

Lors de la mise en œuvre de chaque page, une grande partie de la logique sera répétée (par exemple, accepter une demande, construire l'URL, rendre le modèle et soumettre le HTML résultant). Il est donc pratique d'implémenter un seul ancêtre avec cette logique et de ne l'utiliser que dans les descendants.

Nous commençons par définir l'ancêtre (le nom de la classe n'a pas d'importance, j'utilise une convention du framework Nette) :

```php
abstract class BasePresenter
{
   public function link(string $route, array $params = []): string
   {
      // implémentation de la méthode de construction de l'URL
   }

   public function renderTemplate(string $path, array $params = []): string
   {
      // logique de rendu du modèle
   }
}
```

Lors de la définition de la classe, j'ai utilisé le nouveau mot-clé `abstract`, qui indique que la classe `BasePresenter` est abstraite. Cela signifie que nous ne pouvons pas créer une instance de cette classe, et que nous devons seulement l'utiliser pour qu'une autre classe en hérite et l'implémente. L'abstraction présente d'autres avantages utiles, que nous aborderons plus tard. Une classe n'a pas besoin d'être abstraite pour être héritable - c'est juste l'un des paramètres possibles.

Maintenant nous pouvons implémenter une deuxième classe, par exemple `HomepagePresenter` :

```php
final class HomepagePresenter extends BasePresenter
{
   public function run(): void
   {
       // logique de rendu
       $this->renderTemplate('page d'accueil', [
          'contactLien externe' => $this->link('Contact : par défaut'),
       ]);
   }
}
```

Maintenant, vous avez une classe `HomepagePresenter` fonctionnelle. Notez que la classe est `finale`, ce qui signifie qu'elle ne peut plus être héritée, garantissant que les méthodes seront utilisées exactement comme nous les avons spécifiées.

Lorsque nous avons implémenté la classe, nous avons créé une nouvelle méthode `run()` que seul `HomepagePresenter` peut gérer. À l'intérieur de la méthode, nous appelons la méthode `renderTemplate()` et `link()`, que la classe ne contient pas. Mais cela n'a pas d'importance, car le mot-clé `extends` nous indique d'où les méthodes doivent être héritées, et c'est donc ce qui est utilisé.

Grâce à l'héritage, nous avons pu réaliser la réutilisation du code car une fois écrites, les méthodes peuvent être utilisées à plusieurs endroits.

Remplacer l'implémentation d'une méthode spécifique
------------

Très souvent, il peut être utile de remplacer le comportement d'une méthode particulière lors de l'héritage. Par exemple, si nous voulions changer le comportement de la méthode `link()` de l'exemple précédent dans `ContactPresenter`, cela ressemblerait à ceci :

```php
final class ContactPresenter extends BasePresenter
{
   public function run(): void
   {
      // logique de rendu
      echo $this->link('Page d'accueil:default', []);
   }

   public function link(string $route, array $params = []): string
   {
      return 'https://baraja.cz';
   }
}
```

Pour remplacer l'implémentation, il suffit de redéfinir la méthode dans l'enfant et de remplacer le corps de la méthode. L'important est de respecter l'interface et d'implémenter les mêmes arguments d'entrée.

Visibilité de l'héritage
--------------------------

Parfois, nous aimerions cacher certaines méthodes lors de l'héritage et les utiliser uniquement en interne. Sinon, ne les autoriser que lors de l'héritage et non en tant qu'interface publique.

En général, il existe donc quelques règles simples de visibilité. Nous désignons les méthodes par "public", "protégé" ou "privé", et les règles de visibilité sont les suivantes :

- Tout le monde peut appeler les méthodes `public` de n'importe où, c'est-à-dire en créant une instance d'un ancêtre et d'un descendant.
- Seul un ancêtre ou un descendant peut appeler les méthodes `protégées`, mais elles ne peuvent pas être appelées depuis l'interface publique lorsqu'une instance est créée. Ce sont des méthodes internes pour réaliser l'héritage (une application pratique est par exemple la méthode `link()` dans l'exemple précédent).
- Seule la classe actuelle peut appeler les méthodes `private`, indépendamment des paramètres d'héritage et d'interface publique.

Modification de la visibilité au moment de l'exécution
----------------------------

Dans des cas très spécifiques, il peut être utile de modifier la visibilité d'une méthode au moment de l'exécution, puis de l'appeler. Ceci est utilisé, par exemple, par diverses bibliothèques Doctrine.

Pour modifier la visibilité, nous utilisons alors la classe native **ReflectionClass** implémentée par PHP lui-même.
