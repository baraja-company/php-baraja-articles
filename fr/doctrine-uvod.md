Série Doctrine - Introduction
=============================

> id: fbd0461a-53fe-4713-8926-82e31bd1fb9b
> slug:
> 	cs: doctrine-uvod
> 	fr: serie-doctrine---introduction
> 
> publicationDate: '2021-08-27 11:40:00'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: '2bc62308fcb66acda5a09ac95b5eb2c2'

Doctrine est une bibliothèque PHP avancée pour le travail sur les bases de données orientées objet. L'objectif principal de Doctrine est de décrire le schéma de la base de données à l'aide d'entités de données et de manipuler les données d'une manière entièrement orientée objet.

Ce paradigme est appelé ORM (Object-relational mapping), qui est [design-pattern](/design-patterns) pour convertir (envelopper) les données stockées dans une base de données relationnelle en un objet qui peut être utilisé dans un langage orienté objet. Ainsi, pour comprendre et utiliser Doctrine, vous devez connaître au moins les bases de la [programmation orientée objet](/oop).

Pourquoi apprendre la Doctrine ?
------------------------

Il y a de nombreuses raisons :

- Doctrine est la base de données ORM la plus répandue, utilisée par la plupart de la communauté PHP avancée.
- Il simplifiera considérablement la conception de votre application PHP.
- Vous fournissez un moyen cohérent de concevoir, versionner, transférer et sauvegarder le schéma de votre base de données.
- Vous pouvez [obtenir un grand nombre de tables de base de données en téléchargeant le paquet] (https://github.com/baraja-core/shop-product) sans avoir à comprendre et à configurer quoi que ce soit.
- Les relations entre les tables deviennent de véritables entités physiques
- Les sorties de la base de données ne seront pas des tableaux ordinaires non typés, mais vous obtiendrez des objets physiques réels.
- Vous disposez d'un moyen simple d'effectuer plusieurs opérations simultanément dans une seule transaction.
- Vous augmenterez facilement la sécurité et la résilience de vos applications en sachant simplement quand et comment cela se passe, et que cela se passe en toute sécurité.
- Vous obtenez une couche de code et de base de données facilement testable.
- Vous découvrirez un écosystème entier autour de Doctrine qui résout de nombreux problèmes de manière élégante. Vous trouverez souvent des solutions simples à des problèmes complexes qui, autrement, seraient presque impossibles à résoudre facilement.
- Vous apprendrez beaucoup de nouvelles choses, explorerez de nouvelles idées et utiliserez la base de données à son plein potentiel.
- Débarrassez-vous des requêtes SQL complexes. Doctrine fournit une interface d'écriture de requêtes personnalisées (DQL) qui est très puissante.
- Les applications seront plus rapides. Vous découvrirez facilement les possibilités d'optimisation des applications, tirerez parti du chargement paresseux et trouverez les goulets d'étranglement des applications.

L'opinion de longue date de l'auteur de cet article ([Jan Barasek](https://baraja.cz)) est que Doctrine est le meilleur moyen de travailler avec une base de données PHP. Il n'a tout simplement pas de concurrence.

Comment commencer ?
----------

Avant de commencer à utiliser pleinement Doctrine, vous devez préparer un environnement adéquat. Si vous débutez avec PHP ou si vous n'avez pas de connaissances approfondies, le meilleur choix est d'installer le Nette Framework avec le paquet d'extension [Baraja Doctrine](https://github.com/baraja-core/doctrine), qui intègre automatiquement un support complet. Téléchargez d'abord le paquet via [Composer](/composer), puis configurez l'extension DI et Doctrine commencera à fonctionner automatiquement.

Pour que Doctrine fonctionne correctement, vous devez préparer une base de données vide (Doctrine peut également travailler avec un projet existant, mais cela n'est pas approprié pour les premières étapes car cela risque d'écraser les données existantes) et configurer la connexion. Comme Doctrine n'est pas seulement une bibliothèque de base de données, mais fournit un cadre de base de données avancé, vous devez [résoudre d'autres configurations] (/configure-connections-with-baraja-doctrine). La plupart des paramètres sont automatiquement écrasés dans ce paquet pour Nette, cependant dans la configuration minimale votre serveur doit supporter les extensions `APCu Cache` ou `SQLite3`.

Si tout a été configuré correctement, un nouveau service DI `Baraja\Doctrine\EntityManager` sera créé dans Nette, que vous pouvez [injecter](https://doc.nette.org/cs/3.1/di-usage) dans Presenter :

```php
namespace App\FrontModule\Presenters;

use Baraja\Doctrine\EntityManager;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Si vous parvenez à injecter le service EntityManager de base, vous pouvez commencer à apprendre et à travailler avec Doctrine.

Comment procéder ?
--------

Les chapitres suivants sont une combinaison d'un guide de référence sur la technologie Doctrine, d'années d'expérience, de modèles de conception et de solutions toutes faites. Ensemble, nous passerons en revue tous les éléments de base de Doctrine, de la définition de votre propre entité à la génération d'un schéma de base de données physique, en passant par le travail avec un outil de versionnement et le déploiement en production.

J'utilise Doctrine depuis très longtemps et j'y ai résolu des milliers de cas. Nous montrerons des trucs et astuces sur la manière d'utiliser Doctrine pour optimiser la vitesse de la base de données et sur la manière de concevoir une base de données de manière appropriée. Vous pouvez également utiliser Doctrine pour un projet existant (si vous remplissez certaines conditions) et nous allons vous montrer comment faire.

Cette série d'articles a été créée pour aider mes étudiants en formation et en conseil. Si vous avez besoin de discuter ou d'expliquer certains sujets plus en détail, vous pouvez m'envoyer un courriel à jan@barasek.com. Comme il s'agit d'une technologie relativement exigeante, toutes les questions seront traitées comme une consultation payante.
