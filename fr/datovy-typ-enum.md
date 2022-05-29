Type de données Objet Enum en PHP
=================================

> id: '5b96765c-16c2-4f76-8e31-6de6e9f59365'
> slug:
> 	cs: datovy-typ-enum
> 	de: type-php
>
> publicationDate: '2022-05-29 15:30:00'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: '43735e926db87d2cc6c538ddb67e0a69'

Depuis PHP 8.1, le type de données Enum peut être utilisé pour définir des valeurs d'énumération exactes pour une liste. Ceci est utile dans les cas où nous savons que la valeur d'une variable ne peut prendre que quelques valeurs spécifiques.

Par exemple, voici comment je stocke les types de notification :

```php
enum OrderNotificationType: string
{
    case Email = 'e-mail';
    case Sms = 'texte';
}
```

En PHP, le type de données Enum est un objet classique qui se comporte comme un type spécial de constante, mais qui possède également une instance qui peut être transmise. Cependant, contrairement à un objet ordinaire, il est soumis à un certain nombre de restrictions.

Différences entre Enum et objets
-----------------------

Bien que les enums soient construits au-dessus des classes et des objets, ils ne prennent pas en charge toutes les fonctionnalités associées aux objets. En particulier, il est interdit aux objets enum d'avoir un état interne (ils doivent toujours être une classe statique).

Une liste spécifique de différences :

- Les constructeurs et les destructeurs sont interdits.
- L'héritage n'est pas pris en charge. Les Enums ne peuvent pas être étendus ou hérités par une autre classe.
- Les propriétés statiques ou d'objet ne sont pas autorisées.
- Le clonage de valeurs spécifiques d'Enum (instances) n'est pas supporté, chaque instance individuelle doit être une instance singleton.
- Les méthodes magiques, sauf celles mentionnées ci-dessous, sont interdites.

Les caractéristiques suivantes des objets sont disponibles et se comportent comme n'importe quel autre objet :

- Méthodes publiques, privées et protégées.
- Méthodes statiques publiques, privées et protégées.
- Constantes publiques, privées et protégées.
- Les Enums peuvent implémenter un nombre quelconque d'interfaces.
- Les attributs peuvent être attachés aux enums et aux cas. Le filtre cible `TARGET_CLASS` inclut les Enums eux-mêmes. Le filtre cible `TARGET_CLASS_CONST` inclut les cas Enum.
- Méthodes magiques `__call`, `__callStatic` et `__invoke`.
- Les constantes `__CLASS__` et `__FUNCTION__` se comportent comme des constantes normales.
- La constante magique `::class` sur le type Enum est évaluée comme le nom complet du type de données, y compris tout espace de nom, exactement comme pour un objet. La constante magique `::class` sur une instance du type `Case` est également évaluée comme type Enum, puisqu'il s'agit d'une instance d'un type différent.

Utilisation d'Enum comme type de données
-----------------------------

Imaginons que nous ayons un enum représentant les types de costumes. Dans ce cas, il suffit de définir le type `Suit` et de stocker les différentes valeurs valides.

On obtient alors une instance de l'option particulière classiquement via une quadratique, comme lorsqu'on travaille avec une constante statique.

Exemple de définition d'une Enum, de son invocation par un type spécifique et de son passage à une fonction :

```php
enum Suit
{
	case Hearts;
	case Diamonds;
	case Clubs;
	case Spades;
}

function doStuff(Suit $s)
{
	// ...
}

doStuff(Suit::Spades);
```

Comparaison de deux valeurs
---------------------

L'avantage fondamental des enums par rapport aux objets et aux constantes est qu'il est facile de comparer leurs valeurs.

Une comparaison de base selon laquelle nous travaillons avec une valeur spécifique peut être effectuée comme suit :

```php
$a = Suit::Spades;
$b = Suit::Spades;

$a === $b; // vrai
```

Très souvent, nous devons également décider qu'une valeur particulière appartient à une énumération de valeurs Enum valide. Ceci peut être facilement vérifié comme suit :

```php
$a = Suit::Spades;

$a instanceof Suit;  // vrai
```

Lecture de la valeur du type
---------------------

Nous pouvons lire une valeur de type spécifique soit comme un nom d'une constante d'appel, soit directement comme une valeur réelle définie (si elle existe) :

```php
enum Colors
{
	case Red;
	case Blue;
	case Green;

	public function getColor(): string
	{
	    return $this->name;
	}
}

function paintColor(Colors $colors): void
{
	echo "La peinture :" . $colors->getColor();
}
```

La valeur de la constante d'appel est lue via la propriété `name`. Il est également important de pouvoir implémenter une fonction personnalisée directement dans le type de données Enum, qui peut être appelée sur chaque Enum.

Si un Enum particulier implémente également des valeurs réelles (qui sont cachées sous chaque constante), leur valeur peut également être lue :

```php
enum OrderNotificationType: string
{
    case Email = 'e-mail';
    case Sms = 'texte';
}

$type = OrderNotificationType::Email;

echo $type->value;
```

Toutes les valeurs d'Enum valides
-----------------------------

Nous avons souvent besoin de lister (par exemple, pour l'utilisateur dans un message d'erreur) toutes les valeurs possibles que peut prendre une Enum. Lorsque l'on utilisait des constantes, cela n'était pas possible, Enum le permet facilement :

```php
Suit::cases();
```

Retourne `[Suite::Coeurs, Suite::Carreau, Suite::Trèfle, Suite::Pique]``.

Vérifiez que la variable est de type Enum.
---------------------------------

Nous pouvons facilement vérifier qu'une variable inconnue particulière contient un Enum par une condition :

```php
if ($haystack instanceof \BackedEnum) {
```

Chaque objet Enum est automatiquement un enfant de l'interface générique ``BackedEnum``.

Pour plus d'informations, voir la discussion sur le GitHub de PhpStan : https://github.com/phpstan/phpstan/issues/7304.
