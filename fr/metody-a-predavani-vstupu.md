Méthodes d'EPI et de transfert d'intrants
=========================================

> id: '843fbfb4-daf2-4c2e-9d94-28d494025b2e'
> slug:
> 	cs: metody-a-predavani-vstupu
> 	fr: methodes-d-epi-et-de-transfert-d-intrants
> 
> publicationDate: '2020-02-16 20:49:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3e1bca690ed70479ef9807a1f2a1f23

Les méthodes représentent le comportement d'un objet car elles vous permettent de travailler avec son état interne ainsi que d'influencer les objets entre eux.

Représentation des méthodes dans le monde réel
----------------------------------

Considérez n'importe quel objet du monde réel, disons un chat. Un chat a certaines propriétés (nom, couleur, poids, ...) que nous décrivons à l'aide de propriétés et ensuite aussi un comportement (miauler, marcher, dormir, ...) que nous décrivons à l'aide de méthodes. Puisqu'il y a beaucoup de chats dans le monde ("Et ainsi j'aboie comme le tonnerre, qu'il y ait un million de chats"), il est important de se rappeler qu'une méthode est quelque chose de général qui s'applique à tous les objets d'un type donné de manière égale.

Mise en œuvre pratique d'une méthode
-----------------------------

En termes de syntaxe du langage, définissons une classe pour un chat :

```php
class Cat
{
    public string $name;

    public string $sound = 'Meow';
}
```

Après avoir créé une instance d'un chat particulier, nous pouvons simplement énumérer, par exemple, le son :

```php
$cat = new Cat;

echo $cat->sound; // dit "Miaou"
```

Mais que faire si l'on veut formater le son d'une certaine manière en l'écrivant ? C'est alors que les méthodes entrent en jeu :

```php
class Cat
{
    public string $name;

    public string $sound = 'Meow';

    public function getFormattedSound(): string
    {
        return 'Je fais du bruit "' . $this->sound . '" !';
    }
}
```

Vous devez déjà connaître le principe des fonctions du passé. Les classes permettent d'écrire une fonction directement dans son corps avec une accessibilité définie (comme pour les propriétés) et sont appelées méthodes.

La variable `$this` se comporte un peu "magiquement". Il stocke l'instance actuelle de l'objet dans lequel nous nous trouvons. Si nous voulons lire la valeur d'une propriété ou appeler une autre méthode à l'intérieur d'une méthode, nous le faisons simplement sur la variable `$this`.

La méthode est alors appelée depuis l'intérieur de l'objet comme une fonction classique (avec une parenthèse à la fin) pour indiquer qu'il ne s'agit pas d'une propriété :

```php
$cat = new Cat;

echo $cat->sound; // dit "Miaou"

echo $cat->getFormattedSound(); // imprime "Je fais un bruit de miaou" !
```

Constructeur - méthode appelée lors de la création d'une instance
--------------------------------------------------

Lors de la création d'une instance d'objet, nous devons souvent définir comment définir son état de base et quels paramètres (données d'entrée) sont obligatoires.

Dans la POO pour résoudre ce problème il y a une méthode publique spéciale `__construct` que nous pouvons implémenter volontairement et qui est toujours et seulement appelée lors de la création d'une instance.

Exemple pratique :

```php
class Cat
{
    public string $name;

    public string $sound;

    public function __construct(string $name, string $sound)
    {
        $this->name = $name;
        $this->sound = $sound;
    }

    public function getFormattedSound(): string
    {
        return 'Je fais du bruit "' . $this->sound . '" !';
    }
}
```

En définissant le constructeur, nous nous sommes assurés que lors de la création d'une instance, nous devrons toujours passer 2 paramètres obligatoires (`name` et `sound`) et l'objet sera directement défini.

Comme le constructeur est une méthode classique, il peut faire autre chose avec les données insérées à l'intérieur. Par exemple, reformater la chaîne de manière appropriée, mais nous y reviendrons plus tard.

La création d'une instance est alors à nouveau facile, il suffit de passer les paramètres initiaux (ils sont écrits entre parenthèses au nom de la classe où le nom de la classe est appelé et l'instance est créée) :

```php
$cat = new Cat('Minda', 'Vrr');

echo $cat->name; // C'est écrit "Minda".
echo $cat->sound; // imprime "Vrr"

echo $cat->getFormattedSound(); // imprime "Je fais un son "Vrr" !".
```

Gettery et settery
-----------------

Certaines méthodes sont appelées ``geters`` et ``setters``. Ce sont des méthodes courantes, c'est juste une convention de les appeler ainsi. Les méthodes sont utilisées pour obtenir et insérer des données dans un objet. Le principal avantage est que nous pouvons valider les données avant de les insérer et éventuellement les corriger ou rejeter les données et lancer une erreur.

Pour chaque propriété qui peut être récupérée (nous voulons permettre la récupération des données), il est habituel de créer un getter commençant par le mot `get`. Si la propriété renvoie un booléen (`vrai` ou `faux`), il est d'usage de nommer le début du nom de la méthode avec le mot `s`.

Exemple simplifié :

```php
class Cat
{
    public string $name;

    public function __construct(string $name)
    {
        $this->setName($name);
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function isEmpty(): bool
    {
        return $this->name === '';
    }

    public function setName(string $name): void
    {
        $this->name = trim($name);
    }
}
```

Lors de la création d'une instance de chat, nous passons son nom au constructeur (qui est toujours obligatoire). Cependant, puisque nous voulons partager la logique d'insertion du nom à la fois pour le constructeur et le setter (la méthode d'insertion d'un nouveau nom), nous appelons la méthode `setName()` à l'intérieur du constructeur pour nous assurer que le nom est inséré.

À l'intérieur du setter, nous utilisons la fonction <a href="/function-trim">trim()</a>, qui supprime automatiquement les espaces blancs des deux côtés de la chaîne insérée.

Nous pouvons utiliser la méthode `getName()` pour la sortie. La méthode `isEmpty()` peut être utilisée pour vérifier la vacuité.

> **Avantages pratiques:**
>
> Un énorme avantage des getters et setters sont les **types de données garantis**. Si un getter prétend retourner une `chaîne`, nous pouvons toujours nous y fier et obtenir une chaîne.
>
> Un setter dans son implémentation prétend également accepter une `string`, ce qui signifie pour l'application que tout ce que l'utilisateur saisit sera soit converti en `string` (s'il utilise un type compatible, tel que `integer`), soit recevra une erreur indiquant que le type ne peut pas être converti (tel que `array`).

La plus grande valeur ajoutée des getters et setters sera appréciée surtout dans l'utilisation pratique.

La méthode magique ``toString()``
-----------------------------

Au sein d'une classe, vous pouvez définir une méthode magique `__toString()` qui est automatiquement appelée lorsque vous essayez d'écrire un objet sous forme de chaîne. La méthode doit toujours renvoyer une chaîne de caractères.

Exemple de mise en œuvre :

```php
class Cat
{
    public string $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function __toString(): string
    {
        return 'Bonjour, je suis' . $this->name . ';)';
    }
}
```

L'utilisation est alors la suivante :

```php
$cat = new Cat('Minda');

echo $cat; // Il dit "Salut, je suis Minda ;)".
```

Résumé
-------

Nous avons montré comment définir des méthodes et les appeler dans une instance d'objet.

La prochaine fois, nous examinerons le <a href="/encapsulation">principe d'encapsulation</a>, qui tire pleinement parti des propriétés des méthodes.
