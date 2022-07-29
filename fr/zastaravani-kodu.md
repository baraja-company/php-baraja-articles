Obsolescence du code - comment maintenir la compatibilité
=========================================================

> id: '7837ee7e-a150-47bf-a338-2f4c03d5bbed'
> slug:
> 	cs: zastaravani-kodu
> 	fr: obsolescence-du-code-comment-maintenir-la-compatibilite
> 
> publicationDate: '2022-07-29 17:10:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '727b60c7258fb70e21b5acb445404db0'

Dans le développement de grands systèmes (par exemple, des applications d'entreprise, des progiciels partagés, des bibliothèques, ...) où plusieurs couches et développeurs communiquent entre eux, le problème de la gestion de la publication de nouvelles versions du code se pose.

Prenons l'exemple d'une situation où nous voulons développer un paquet Composer partagé pour une communauté de développeurs.

Version sémantique
--------------------

Avant de résoudre le problème de la compatibilité ascendante et descendante, nous devons trouver comment suivre les modifications apportées au logiciel. Actuellement (2022), la meilleure façon de versionner tous les changements est dans Git. Le dépôt de logiciels peut être partagé, par exemple, via GitHub ou GitLab. Chaque changement de logiciel a un identifiant unique qui identifie chaque livraison et décrit ce qui s'est réellement passé.

La stratégie suivante a bien fonctionné pour moi lors du développement de bibliothèques :

Au début du développement, un commit initial est créé dans la branche `master` (ou `main`), où la structure du fichier sous-jacent est commise.

Pour chaque nouvelle demande, une branche distincte de master est créée pour y travailler. Lorsque la modification est prête, une demande de fusion est envoyée au maître sous la forme d'une "demande d'extraction". Une revue de code est effectuée sur la demande et si tout est ok, la modification est fusionnée dans le master.

Si la branche contient un changement incompatible avec le passé (BC break, de `Back Compatibility Break`), cela doit être marqué en conséquence. La méthode de marquage des pauses BC est discutée dans les chapitres suivants.

La version de production de la bibliothèque est ensuite balisée à l'aide de balises ayant la structure suivante (basée sur **Semantic Versioning 2.0.0**) :

Nous écrivons le numéro de version dans le format `MAJOR.MINOR.PATCH`. L'incrémentation des numéros de version se fait comme suit :

- `MAJOR` - quand il y a un changement qui n'est pas rétrocompatible avec d'autres (API)
- `MINOR` - lorsque des fonctionnalités sont ajoutées tout en maintenant la rétrocompatibilité.
- `PATCH` - lorsqu'un bogue est corrigé et que la rétrocompatibilité est maintenue.

En utilisant les préversions et en ajoutant des métadonnées, il est possible d'affiner les informations. Par exemple : `1.0.0-alpha`, `1.0.1-beta+2`.

Vous pouvez en savoir plus sur le versionnement sémantique sur le site officiel : https://semver.org.

Compatibilité ascendante et descendante
-------------------------------

Lors de la conception d'un logiciel, vous devez toujours penser à la **compatibilité ascendante** (les nouvelles fonctionnalités et les modifications doivent être compatibles avec l'ancien code) et, dans certains cas, à la **compatibilité descendante** (les fonctionnalités actuelles doivent être compatibles avec les futures modifications de l'interface).

Réussir ces deux tâches est un véritable défi. Il n'est pas toujours possible d'apporter une modification sans rompre la compatibilité.

Lors des modifications, il est toujours nécessaire de procéder par étapes et de laisser aux utilisateurs suffisamment de temps pour réagir aux changements.

Les sections suivantes décrivent comment y réfléchir.

Étape 1 : Marquer une fonctionnalité comme étant obsolète
--------------------------------------

Le type de base de menace de compatibilité est la suppression ou le changement de nom d'une fonctionnalité qui existait dans le passé. Le plus souvent, c'est parce que les arguments que la fonction accepte ont changé, ou qu'il s'agit d'une ancienne logique qui doit être traitée différemment dans la nouvelle manière.

Dans un premier temps, les anciennes parties du code doivent être marquées comme dépréciées mais ne doivent pas être modifiées.

En PHP, il y a une annotation `@deprecated` pour cela, qui doit être écrite directement au-dessus des méthodes, fonctions, propriétés, variables, constantes, et généralement de tout code déprécié.

Il est également bon d'expliquer pourquoi un élément particulier est déprécié et comment il sera modifié à l'avenir. Par exemple, donnez le nom d'une nouvelle fonction ou d'une nouvelle méthode d'utilisation.

Un exemple concret de marquage de code obsolète : Les constantes seront supprimées, il est préférable d'utiliser l'Enum intégrée (BC break dû à la migration vers une version plus récente de PHP) :

```php
class OrderNotification
{
	/** @déprécié depuis 2022-05-24, utiliser l'enum OrderNotificationType */
	public const
		TYPE_EMAIL = 'e-mail',
		TYPE_SMS = 'texte';
```

L'annotation `@deprecated` ne provoquera qu'un avertissement silencieux pour l'IDE (outil de développement) et les outils de compilation. Il ne casse rien.

Phase 2 : Appel d'une nouvelle méthode/logique
--------------------------------------

Dans la deuxième phase, nous remplaçons l'ancienne mise en œuvre par la nouvelle, mais nous utilisons la nouvelle méthode dans l'ancienne mise en œuvre. Cela permettra de maintenir la compatibilité de l'interface sans que l'utilisateur s'en aperçoive.

Exemple : la méthode est dépréciée car un nouveau service statique a été créé à la place. Puisque quelqu'un peut l'utiliser, elle est simplement marquée comme dépréciée et appelle en interne la nouvelle implémentation. Le développeur peut généralement supposer que la méthode sera complètement supprimée à l'avenir.

```php
/** @déprécié depuis 2021-09-11 utiliser Ip::get() à la place. */
public static function userIp(): string
{
	return Ip::get();
}
```

Phase 3 : modifier les annotations pour l'analyse statique
-------------------------------------------

Si vous utilisez une analyse statique comme PhpStan (fortement recommandé !), il est bon de réécrire d'abord les annotations PHPDoc avant de modifier réellement les types de données. L'analyse statique signalera à l'utilisateur que quelque chose ne fonctionne pas, mais le moteur d'exécution restera intact.

Étape 4 : Jeter le préavis
-----------------------

Dans la quatrième phase, une nouvelle méthode est appelée, et une erreur de niveau `note` est lancée en même temps. L'application fonctionne toujours, elle commence simplement à stocker progressivement dans le journal du système des informations indiquant qu'une fonction est dépréciée et qu'elle sera modifiée ou supprimée. Nous allons désormais lancer une alerte active sur ce type de changement. Le développeur verra des erreurs pendant le développement ou la compilation.

```php
/** @déprécié depuis le 2021-05-01, utilisez UserMetaManager à la place. */
public function getMeta(int $userId, string $key): ?string
{
	trigger_error(__METHOD__ . ': Cette méthode est obsolète, utilisez UserMetaManager à la place.');
	return $this->userMetaManager->get($userId, $key);
}
```

Étape 5 : Lancer une exception
------------------------

Je recommande de lancer l'une des exceptions fatales avant de supprimer complètement la méthode. Ceci est particulièrement important car l'application sera complètement arrêtée et l'erreur ne pourra pas être ignorée. Contrairement à la suppression complète du code, l'utilisateur sera informé de ce qui s'est réellement passé et pourra facilement corriger l'erreur.

Étape 6 : suppression complète du code
-----------------------------

Dans la dernière étape, l'ancien code sera complètement supprimé. Si un utilisateur n'a pas corrigé les dépendances, son application sera cassée.

Les ruptures sérieuses de la CB dans des zones sensibles devraient toujours être faites dans la prochaine version `MAJOR` et devraient être signalées au moins une version `MAJOR` plus tôt en lançant un avis. Si vous ne le faites pas, la mise à jour de la bibliothèque sera extrêmement difficile.
