Utilisation non sécurisée des constantes en PHP
===============================================

> id: b4d0f45f-c059-4a47-9b63-8980d0d465ed
> slug:
> 	cs: nebezpecne-konstanty
> 	fr: utilisation-non-securisee-des-constantes-en-php
> 
> publicationDate: '2021-06-22 10:49:46'
> mainCategoryId: '561b0614-e152-4a62-a634-1f2d605d39d9'
> sourceContentHash: '58d4ea2e4bf106402386506b023ba4d2'

Il y a deux choses délicates à garder à l'esprit lorsque vous utilisez des constantes en PHP.

Constantes dynamiques et statiques
------------------------------

Une constante peut être définie en PHP soit de manière statique directement dans la classe (la meilleure solution), par exemple comme suit :

```php
class Region
{
	public const PREFIX = 420;
}
```

Et l'utilisation est très claire. Au moment de la compilation de la classe, la valeur de la constante est décidée et nous pouvons y accéder en appelant le nom de la classe et la constante elle-même. Le plus souvent en écrivant `Region::PREFIX`.

L'autre méthode (bien pire) consiste à définir la constante de façon dynamique au moment de l'exécution (le plus souvent quelque part dans un script de configuration), où il s'agit alors de quelque chose du genre :

```php
define('BASE_DIR', __DIR__ . '/../');
```

L'inconvénient majeur de définir une constante via la fonction `define` est que le script qui définit la constante peut ne pas avoir été appelé, donc la constante n'existera pas lorsqu'on tentera de la lire.

Lorsqu'il est combiné à l'utilisation d'une constante dynamique dans la définition d'une constante statique dans une classe, cela peut même entraîner une erreur de réflexion fatale :

```php
class InvoiceGenerator
{
	// C'est complètement faux !
	public const DATA_DIR = BASE_DIR . '/données/facture';
}
```

> **Explication:**
>
> L'utilisation d'une constante dynamique au sein d'une constante statique présente un inconvénient majeur : la valeur de la constante dynamique ne peut pas être lue au moment de la compilation. Ce script doit donc être traité à nouveau à chaque requête (c'est-à-dire qu'il ne peut pas être stocké dans le cache OPC pour optimiser la vitesse), et si la constante n'existe pas du tout, une erreur de compilation fatale est lancée et l'application ne peut pas s'exécuter du tout.

Si vous utilisez PhpStan, il peut vous avertir automatiquement de ce problème :

```txt
Reflection error: Could not locate constant "BASE_DIR" while
evaluating expression in InvoiceGenerator at line 6
```

> **Learning:**
>
> La valeur de toutes les constantes doit toujours être constante.


Héritage des constantes lors de l'utilisation des statiques
-------------------------------------

Dans certains cas, il est utile d'utiliser l'héritage pour remplacer la valeur d'une constante. Mais dans ce cas, l'ancêtre ne peut pas lire la valeur du descendant (ou ne devrait pas le faire).

Un exemple est la définition des pays et des régions :

```php
abstract class Region
{
	public function getPrefix(): int
	{
		// Erreur fatale !
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

Le paradoxe est que le code ci-dessus ne lève pas nécessairement une erreur, mais il peut être levé par une utilisation inappropriée de l'héritage.

Si nous appelons la méthode `getPrefix()` sur un descendant de `CzechRepublic`, tout sera correct car la valeur de la constante sera lue correctement. Toutefois, si le descendant n'a pas défini la valeur de la constante, une erreur fatale de constante inexistante est déclenchée. Le pire dans tout ça, c'est qu'il s'agit d'une **dépendance cachée** qui est créée dans l'implémentation de la méthode, et le développeur qui hérite de la classe peut ne même pas être au courant de cette dépendance.

La meilleure solution dans ce cas est soit de définir une constante directement dans l'ancêtre avec une valeur par défaut (de sorte que la logique passe toujours), soit au moins de lancer une exception dans le getter.

```php
abstract class Region
{
	public const REGION = null;

	public function getPrefix(): int
	{
		if (static::REGION === null) {
			throw new \LogicException('La région n'a pas été définie.');
		}
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

PhpStan répond à cette erreur comme suit :

```txt
Access to undefined constant static(Region):REGION.
```
