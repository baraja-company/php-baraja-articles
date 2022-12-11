Drapeaux de caractéristiques / interrupteurs d'activation et de désactivation des caractéristiques
==================================================================================================

> id: '4209c32f-655c-46fa-9090-e5ec3883ea61'
> slug:
> 	cs: feature-flagy
> 	fr: drapeaux-de-caracteristiques-interrupteurs-d-activation-et-de-desactivation-des-caracteristiques
> 
> publicationDate: '2022-12-11 15:00:00'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '1e32c312eab2ca16539cdb7431e2dda9'

Lors du développement d'une application plus complexe, vous apprécierez la possibilité de développer davantage de fonctionnalités dès le départ, de les distribuer avec la prochaine version de votre logiciel et d'activer la fonctionnalité ultérieurement.

C'est exactement pour cela que les drapeaux de caractéristiques ont été créés. Cet article vous montre comment les utiliser.

Mise en œuvre de base
---------------------

Les indicateurs de fonctionnalité sont un concept très simple qui consiste à appeler une seule fonction/méthode qui décide si une nouvelle fonctionnalité est active.

Par exemple :

```php
echo '<h1>Applications météo</h1>';
echo 'Aujourd'hui, c'est le cas :' . getWeather();

if (feature('carte')) {
   echo 'Carte :' . getMap();
}
```

Pour vérifier la disponibilité d'une actualité particulière, la fonction `feature()` est appelée, qui décide d'autoriser ou d'ignorer la fonctionnalité particulière en fonction du nom de l'appel.

Mise en œuvre de la logique de décision
-------------------------------

La logique de décision est souvent complexe. Par exemple, vous ne pouvez exécuter une fonction spécifique qu'à partir d'une date précise, ou pour les utilisateurs d'un groupe spécifique. Par exemple, je teste souvent le déploiement d'une nouvelle fonctionnalité sur, disons, 5 % des utilisateurs de cette manière afin qu'elle n'affecte pas tout le monde en même temps.

Par exemple, lors du développement d'un logiciel d'entreprise, c'est ainsi que nous organisons des campagnes publicitaires et des réductions qui sont valables à partir d'une certaine date.

Si une nouvelle fonctionnalité particulière se casse la figure, il est possible de la désactiver simplement avec un indicateur de fonctionnalité pour les utilisateurs, et de l'activer pour un groupe de développeurs qui la testeront et apporteront un correctif, par exemple.
