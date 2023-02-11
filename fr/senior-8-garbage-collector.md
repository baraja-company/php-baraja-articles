Utilisation inappropriée du Garbage Collector
=============================================

> id: '842e30ab-1af1-4152-ba0f-3004ca3b620f'
> slug:
> 	cs: senior-8-garbage-collector
> 	fr: utilisation-inappropriee-du-garbage-collector
> 
> publicationDate: '2023-02-11 14:35:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: ae5993313436e0d7c5291b6337b988ef

Vous êtes le développeur d'une importante application patrimoniale, dans laquelle vous introduisez progressivement PHPStan. On commence par le niveau 0, qui est assez difficile, mais on finit par y arriver. Vous passez aux niveaux suivants, où une partie de votre code commence à signaler une variable $lock inutilisée que vous devriez supprimer.

Le code ressemble à ceci :

```php
public function processOrder(int $orderId): void
{
	$lock = Lock::createLock('commande-' . $orderId);

	// Il y a une certaine logique ici...
}
```

Vous vous dites qu'il doit y avoir un verrou stocké dans la variable que quelqu'un a oublié de libérer plus tard, ou peut-être que cela se produit dans d'autres méthodes qui sont appelées plus tard. Vous décidez donc de supprimer la variable inutilisée, en ne conservant que l'appel à la méthode statique qui crée le verrou.

Cette décision pourrait-elle provoquer une erreur critique ?

Si oui, pourquoi, et comment le mécanisme original aurait-il pu fonctionner ?

Si non, pourquoi pas, et comment savez-vous que cette opération est toujours sûre ?
