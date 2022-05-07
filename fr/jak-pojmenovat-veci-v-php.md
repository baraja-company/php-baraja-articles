Comment nommer les variables, les fonctions, les méthodes et les classes ?
==========================================================================

> id: f70ac75d-422f-4696-88d5-9e1b843e060a
> slug:
> 	cs: jak-pojmenovat-veci-v-php
> 	fr: comment-nommer-les-variables-les-fonctions-les-methodes-et-les-classes
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: '10510114a0160c0826c9ee1f54c95b03'

Pour garder le code en ordre, il est important de choisir des règles claires sur la façon dont nous dérivons les noms. Cette page sert d'aperçu des approches relativement populaires utilisées par un grand nombre de programmeurs, dont moi-même et les personnes avec lesquelles je travaille.

Si vous travaillez dans une équipe de développement, utilisez leurs règles, mais pour le développement général, il est toujours utile d'établir quelques bonnes habitudes.

Le concept de la syntaxe PHP étant très vaste, j'ai divisé le guide en plusieurs catégories très spécialisées.

Si vous cherchez une solution rapide, je vous recommande d'étudier <a href="https://www.php-fig.org/psr/psr-4/">la norme PSR-4</a>.

Insertion de scripts, liens vers le HTML, chargement de fichiers
---------------------------------------------------

Chaque script doit commencer par la balise `<?php`.

S'il n'y a **pas de HTML** à la fin du fichier, celui-ci ne doit pas être terminé (pour éviter un caractère blanc à la fin de la page).

Le chargement des autres fichiers doit suivre les règles suivantes :

- Si le fichier n'est pas critique et peut être ignoré lors du rendu de la page (par exemple, des informations supplémentaires dans le pied de page), il doit être chargé via include : `include 'file.php';`
- Fichiers critiques uniquement via la construction require : `require 'file.php';`
- Si nous travaillons avec des objets, nous utilisons la construction require pour charger l'autoloader, qui se chargera lui-même de charger les autres classes (la plupart des frameworks implémentent ce comportement).


Si cela est possible, nous devrions séparer la logique d'extraction des données du rendu en HTML, c'est-à-dire utiliser le modèle MVC (modèle - vue - contrôleur).

Nous préparons donc d'abord les données dans, disons, Presenter :

`Presenter.php`

```php
$cisla = [1, 2, 3];

$data = [];
$data['numéros'] = $cisla; // passer les données au modèle
include 'renderCisel.php'; // charger le modèle
```

Et ensuite le dessiner dans le modèle :

`renderCisel.php`

```html
<table>
<?php
    foreach ($data['cisla'] as $cislo) {
        echo '<tr><td>' . $cislo . '</td></tr>';
    }
?>
</table>
```

Cette approche est utilisée par la plupart des frameworks et est utile pour améliorer la clarté du code. Dans la dernière partie du processus de développement, cette approche devrait être utilisée par tout programmeur expérimenté qui souhaite développer des applications clairement structurées (pour les grandes applications, cette approche est indispensable).

Noms des variables
----------------

Si une variable contient un tableau de valeurs ou d'autres objets, elle doit être nommée au pluriel :

```php
$numbers = [1, 2, 3];
```

Parce qu'alors nous pouvons simplement itérer les valeurs par un seul nombre :

```php
foreach ($numbers as $number) {
    // traitement des numéros
}
```

Un nom composé de plusieurs mots est combiné en un seul mot long avec la syntaxe cameCase, c'est-à-dire que le premier mot commence par une minuscule, chaque mot suivant par une majuscule :

```php
$promenna = 'Hé ! Hé !';
$seznamUzivatelu = [
   'Jan Barášek',
   'Barack Obama',
   'Steve Jobs',
   'Stephen Wolfram',
];
$maxFilesInDirectory = 12;
$nameOfPhpScript = 'index.php'; // La taille de l'abréviation PHP a été réduite.
```

Noms de fonctions et de méthodes
--------------------

Les fonctions et les méthodes doivent toujours indiquer clairement dans leur nom ce qu'elles font. Souvent, les paramètres d'entrée et la valeur de retour attendus peuvent également être inclus dans le nom.

Essayez de deviner ce que font les fonctions suivantes et quelle est leur valeur de retour :

```php
getUserById($id);
saveErrorToLog($message);
createDefaultDirectory($path);
setAuthors(['Jan Barášek', 'Chuck Norris']);
getCurrentTime();
```

L'astuce réside dans le premier mot du nom, qui indique clairement la méthode utilisée par la fonction. La convention suivante est généralement respectée :

- `get` - récupère les données sous forme de tableau ou d'objet, les paramètres d'entrée spécifient l'entité recherchée.
- `save` - enregistrer dans un fichier ou une base de données
- `create` - créer une entité (par exemple, créer une instance d'un objet)
- `set` - sauvegarde des données dans une variable prédéfinie (à l'intérieur d'une fonction)

Noms des classes
----------

Une classe est une grande entité qui contient un grand nombre de propriétés et de méthodes, elle doit donc également commencer par une majuscule. Une classe ne doit également porter qu'une seule entité (et décrire ses propriétés), elle doit donc être nommée avec un nom singulier. Si nous devons travailler avec plusieurs entités, nous pouvons simplement stocker chaque instance dans un tableau.

Exemple :

```php
class User
{
    public string $username;

    public string $password;

    public string $role;
}

class Users
{
    /** @var User[] */
    public array $users;

    public function addUser(User $user): void
    {
        $this->users = array_push($this->users, $user);
    }
}
```

La classe **User** est spécialisée dans les informations concernant un seul utilisateur spécifique. Si nous voulons travailler avec plusieurs utilisateurs, nous créons une autre classe (enveloppe) qui porte un tableau d'instances d'une entité spécifique.

Les fabriques peuvent aussi souvent être utiles à cet effet, car elles nous permettent de créer facilement des objets similaires et de recycler les instances originales, ce qui donne un code plus clair tout en économisant les ressources du système.

Espace de nommage - Espaces de nommage
---------------------------

Bien que Namespace soit indépendant du répertoire physique où le script est disponible, c'est une bonne pratique de respecter au moins partiellement la disposition du projet (ce qui conduit à un meilleur système de création de nouveaux noms qui est plus non ambigu de cette façon).

Personnellement, je nomme les espaces de noms en fonction du sous-répertoire commun aux classes de ce type.

Exemples :

```php
App\Presenters; // Ce sont les noms de tous les présentateurs.
App\Model;      // C'est le nom du modèle général
App\Model\Math; // C'est le nom du modèle qui travaille avec les mathématiques.
```

Pour un chargement automatique correct des classes, il est bon de suivre la norme <a href="https://jakpsatphp.cz/PSR4/">PSR-4</a>.

Conventions personnalisées (par exemple, dans une équipe d'entreprise)
-----------------------------------------

Et comment tu appelles le tien ? J'apprécierais des conseils sur la façon d'améliorer cet article.

En général, cependant, les conventions personnalisées au sein d'une équipe n'ont pas beaucoup de sens, car elles rendent le code plus difficile à porter vers d'autres frameworks et vous devez apprendre les conventions actuelles lorsque vous embauchez un nouveau collègue. Il est donc préférable de suivre la norme `PSR-4`.
