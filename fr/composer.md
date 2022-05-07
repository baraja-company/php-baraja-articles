Composer - aperçu complet des fonctions avancées
================================================

> id: a74d8d59-91ce-4602-ad52-80cf89a647bd
> slug:
> 	cs: composer
> 	fr: composer---apercu-complet-des-fonctions-avancees
> 
> perex: Composer je pokročilý správce balíků a závislostí pro vaše PHP aplikace. Článek popisuje jeho všechny výhody a možnosti použití.
> publicationDate: '2020-03-10 20:18:19'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '68340d4b4d3c8a6ed143ede176fbf04e'

Comme vous le savez déjà, [Composer] (https://getcomposer.org/) est un gestionnaire de paquets et de dépendances robuste pour PHP, grâce auquel vous pouvez gérer élégamment des centaines de projets à la fois et distribuer le code une fois écrit à toutes les applications simultanément.

Ce tutoriel sert de guide détaillé et complet pour les développeurs. Nous couvrirons toutes les techniques avancées importantes pour travailler avec Composer, tout en expliquant les détails techniques et les dépendances pertinentes.

Installation de Composer
-------------------

Quelle que soit la plate-forme, nous téléchargeons à partir du [site officiel du compositeur] (https://getcomposer.org/).

En interne, le fichier PHP `composer-setup.php` est téléchargé, qui lorsqu'il est exécuté en mode CLI peut installer Composer. Il est également important de savoir que Composer ne fonctionne pas sans PHP. Vérifiez donc d'abord que PHP fonctionne sur votre ordinateur (il doit seulement être disponible pour le terminal).

Sur Mac et Linux, Composer fonctionne dès son installation et il suffit d'appeler la commande `composer -v` pour vérifier rapidement que Composer est installé correctement.

Sous Linux, la commande suivante peut être utilisée pour installer : `/usr/bin/php -r "copy('https://getcomposer.org/installer', 'composer-setup.php') ;"`.

Sous Windows, il est conseillé d'installer l'outil [Git Bash for Windows](https://gitforwindows.org/), qui vous permet d'ouvrir un terminal spécifique qui se comporte presque comme Linux et vous permet de travailler dans le même environnement que sous Linux.

L'installation pour les serveurs est la même que sur un environnement local. Assurez-vous simplement que vous disposez de la bonne version de PHP que Composer utilise en interne.

Commandes disponibles
----------------

Composer met en œuvre un certain nombre de commandes.

L'utilisation est : `composer <commande>`, par exemple : `composer update`.

Aperçu pour `1.10.0` :

| Commande | Description / Signification |
|-----------------------|----------------|
| `about` | Affiche de brèves informations sur Composer. | |
| `archive` | Crée une archive avec le contenu du paquet Composer sélectionné. | `archive` | Crée une archive avec le contenu du paquet Composer sélectionné.
| `browse` | Ouvre la page d'accueil du paquet sélectionné, de l'auteur ou d'une autre page connexe dans un navigateur web. Il peut souvent contenir une documentation sur la façon de l'utiliser. |
| `cc` | Vide le cache interne de Composer avec les versions des paquets téléchargés dans le passé. |
| `check-platform-reqs` | Vérifier si les conditions d'installation sont remplies pour la plateforme actuelle. | |
| `clear-cache` | Effacer le cache interne de Composer. |
| `clearcache` | Effacer le cache interne de Composer. |
| | `config` | Définit une directive de configuration.
| `create-project` | Crée un nouveau projet basé sur le paquet sélectionné et crée automatiquement un dossier dans lequel placer le projet.
| `depends` | Montre quels paquets ont causé l'installation du paquet sélectionné. | |
| `diagnose` | Diagnostique le système pour identifier les erreurs courantes. Le traitement de la sortie est à la charge du développeur, il s'agit juste d'un listing. |
| `dump-autoload` | Génère un nouveau <a href="/autoloading-trid">autoloader</a> .
| `dumpautoload` | Génère un nouveau <a href="/autoloading-trid">autoloader</a> .
| `exec` | Exécute les binaires et les scripts du vendeur. | |
| `fund` | Explore comment faire/modifier vos dépendances. | |
| `global` | Vous permet d'exécuter les commandes globales de Composer à partir de la variable `$COMPOSER_HOME` .
| `help` | Imprime l'aide pour les commandes. |
| `home` | Ouvre la page d'accueil d'un paquet spécifique dans le navigateur. |
| `i` | Installe toutes les dépendances du projet en fonction du fichier `composer.lock`, s'il existe et s'il est valide. S'il y a un problème, les informations provenant de `composer.json` sont utilisées et `composer.lock` est rétabli à son état initial. |.
| `info` | Affiche des informations sur les paquets actuellement installés dans le projet. Affiche les noms de tous les paquets, leurs versions actuelles et une courte description. |
| `init` | Crée une fonction de base `composer.json` dans le répertoire courant. | |
| `install` | Installe toutes les dépendances du projet en fonction du fichier `composer.lock`, s'il existe et s'il est valide. Si un problème survient, les informations provenant de `composer.json` sont utilisées et `composer.lock` est ramené à son état d'origine. |.
| `licenses` | Affiche une liste de tous les paquets, leurs versions et leur licence actuelle. | |
| `list` | Affiche une liste des commandes disponibles. | |
| | `outdated` | Affiche une liste de tous les paquets pour lesquels il existe une version plus récente disponible pour l'installation et qui répondent aux dépendances. Pour chaque paquet, la dernière version compatible que Composer suggère pour l'installation sera affichée. | |
| `prohibits` | Indique les paquets et les dépendances qui empêchent l'installation du paquet ou de la version demandée. | |
| `remove` | Supprime le paquet de la section de configuration `require` ou `require-dev`. | |
| `require` | Ajoute le paquet demandé à `composer.json` et l'installe. Si les dépendances ne peuvent être satisfaites, il reviendra à l'état d'origine. |
| `run` | Exécute les scripts définis dans le fichier `composer.json`. | | `run` | Exécute les scripts définis dans le fichier `composer.json`.
| `run-script` | Exécute les scripts définis dans `composer.json`. |
| `search` | Recherche des paquets par mot-clé ou par requête de recherche. |
| `self-update` | Met à jour la dernière version du fichier interne `composer.phar` .
| `selfupdate` | Met à jour la version interne `composer.phar` à la dernière version. |
| | `show` | Affiche des informations détaillées sur les paquets actuellement installés. | | | `show` | affiche des informations détaillées sur les paquets actuellement installés.
| `status` | Affiche un résumé des changements locaux effectués manuellement sur les paquets, basé sur une comparaison avec la source du paquet à partir duquel il a été installé à l'origine. | |
| `suggests` | Affiche les suggestions de paquets. Les suggestions peuvent inclure différents types d'actions, comme l'installation de mises à jour de sécurité, etc.
| `update` | Met à jour l'ensemble du projet en fonction des dépendances, afin qu'elles soient toujours toutes satisfaites par `composer.json`. Si elle réussit, elle met à jour `composer.lock`, où elle écrit les versions actuellement installées. |
| `upgrade` | Alias pour `update`. |
| `u` | Alias vers `update`. |
| `validate` | Vérifie `composer.json` et `composer.lock` pour des erreurs de syntaxe. | |
| `why` | Montre quels sont les paquets qui ont causé l'installation du paquet actuellement sélectionné, y compris toutes les dépendances. | |
| `why-not` | Affiche les paquets et les versions qui empêchent l'installation du paquet ou de la version sélectionné(e). | |

Créer et définir un projet
----------------------------------

Chaque projet géré par Composer est défini par un fichier `composer.json` à sa racine qui définit toutes les dépendances. Le fichier peut être créé soit manuellement pour un projet existant, soit automatiquement lors de la création d'un projet.

Comme tout dans Composer est un paquet, le projet lui-même peut être basé sur un paquet. Ainsi, par exemple, si vous créez des dizaines ou des centaines de projets très similaires, il est judicieux de placer leur configuration et leur structure de base dans un paquetage distinct sur lequel fonder l'installation.

Un exemple d'un tel paquet est mon [Baraja Sandbox](https://github.com/baraja-core/sandbox), qui est basé sur la pure Nette 3.0 et ajoute une dépendance de base à mon [Package Manager](https://github.com/baraja-core/package-manager), que j'utilise pour tous les projets et la gestion des dépendances dans la configuration Nette.

Le bac à sable est ensuite installé simplement avec la commande :

```shell
$ composer create-project baraja/sandbox <your-project-name>
```

Sur la base du nom du projet, Composer créera alors automatiquement un dossier pour installer le projet (copier le contenu du paquet et installer les dépendances).

Dans le dossier `vendor`, Composer gère alors tous les paquets (leurs fichiers physiques sont là) et génère un autoload de classes, que nous mettons idéalement directement dans `index.php` comme une ligne :

```php
require __DIR__ . '/vendor/autoload.php';

// Le code de l'application elle-même
```

Installation de paquets et de dépendances supplémentaires
-------------------------------------

Dans un projet fonctionnel, nous pouvons installer de nouveaux paquets et ajouter des dépendances très facilement.

Il y a deux façons de procéder :

- Avec la commande `composer require ...`,
- En ajoutant une dépendance directement au fichier `composer.json` dans la section `require`, puis en utilisant la commande `composer update`.

Essayez d'installer par exemple PackageManager : `composer require baraja-core/package-manager`, ou [Doctrine](https://github.com/baraja-core/doctrine) : `composer require baraja-core/doctrine`.

Si le paquet choisi ne peut pas être installé, des raisons spécifiques peuvent être demandées et Composer listera les dépendances qui l'empêchent. Il suffit souvent de corriger une dépendance à une version particulière ou de supprimer un code incompatible. Pour des informations détaillées, utilisez la commande : `composer pourquoi baraja-core/doctrine`.

Mise à jour des projets et des paquets
-----------------------------

Un projet bien conçu est développé de manière à ce que vous puissiez facilement télécharger les mises à jour au fil du temps et disposer toujours des dernières versions de tous les paquets. Le principal avantage est que vous bénéficiez de la correction de tous les bogues, d'améliorations fréquentes des performances et de nombreuses nouvelles fonctionnalités. En outre, la transition progressive rendra la mise à jour moins compliquée après une longue période, car vous réglerez les problèmes à la volée à une échelle plus réduite et éviterez les incompatibilités.

Pour mettre à jour tous les paquets et dépendances, utilisez la commande `composer update`.

Le processus de mise à jour lui-même peut échouer dans certains cas. La raison en est généralement une dépendance brisée ou un paquet incompatible.

Pour des informations détaillées sur les raisons pour lesquelles un paquet ne peut pas être installé, utilisez la commande : `composer why-not baraja-core/doctrine`. Si nous avons déjà le paquet et que seule la version spécifique ne fonctionne pas (elle ne veut pas s'installer), nous pouvons aussi demander la version spécifique : `composer why-not baraja-core/doctrine:v1.0.20`.

Dans le fichier `composer.json`, nous pouvons également lister la dépendance à un runtime spécifique. Ceci est particulièrement utile lorsque nous développons un projet avec plusieurs personnes et que nous voulons vérifier qu'elles ont toutes les extensions installées.

Typiquement, la version de PHP est vérifiée (doit être `7.1.0` ou plus récent) :

```json
{
   "require": {
      "php": ">=7.1.0"
   }
}
```

Eventuellement d'autres extensions du système :

```json
{
   "require": {
      "php": ">=7.1.0",
      "ext-json": "*",
      "ext-session": "*",
      "ext-PDO": "*"
   }
}
```

Ces règles sont ensuite prises en compte lors de l'installation de paquets ou de mises à jour. Cela permet d'éviter des problèmes qui deviendraient apparents au moment de l'exécution. Typiquement, par exemple, un paquet de passerelle de paiement doit communiquer avec l'API, il doit donc admettre des dépendances sur les extensions `curl` et `json`.

Dépannage des dépendances
-----------------------------

Souvent, les violations de dépendances se produisent à cause d'une mauvaise version de PHP. Dans ce cas, Composer envoie un message, par exemple : "votre version de PHP (7.3.11), remplacée par la version de "config.platform.php" (7.1), ne répond pas à cette exigence".

Très souvent l'erreur est causée par les paramètres directement dans le projet `composer.json`, où se trouve la section suivante :

```json
"config": {
   "platform": {
      "php": "7.2"
   }
}
```

La modification **doit être effectuée directement dans le fichier**. Dans le cas d'un projet global (avant l'installation ou avec une dépendance globale), vous pouvez forcer la version de Composer avec `composer config -g platform.php 7.2.14` (le commutateur `-g` signifie `global`).

Dans certains cas, nous voulons installer les dernières versions des paquets et ignorer les paramètres de l'environnement local. Dans ce cas, nous pouvons utiliser la commande avancée :

```shell
$ composer update --ignore-platform-reqs
```

**Utilisez le commutateur `ignore-platform-reqs` à vos risques et périls, il peut causer des dommages au projet!** Ne l'utilisez pas si vous ne comprenez pas les conséquences.

Appels, paramètres et gestion de la mémoire de Manual Composer
------------------------------------------------------

Composer est en fait un script PHP enveloppé dans un PHAR, qui est une version compilée d'une application PHP. La connaissance de ces informations peut être mise à profit, par exemple pour mieux paramétrer l'appel lui-même.

Dans les projets de grande envergure, il arrive parfois que nous manquions de RAM et que nous devions en allouer beaucoup plus, ou augmenter le temps de traitement des scripts.

Par exemple, sous Windows, vous pouvez utiliser cette commande pour cela :

```shell
$ php -d memory_limit=-1 C:/xampp/htdocs/composer.phar update
```

Le commutateur `memory_limit=-1` indique à Composer de ne pas être sensible à la limite de RAM et de consommer autant de mémoire que nécessaire.

Scripts utilisateur personnalisés après une action Composer
--------------------------------------------

Après avoir exécuté des actions Composer, vous pouvez invoquer l'exécution automatique de scripts définis par l'utilisateur qui effectuent des actions spécifiques sur le projet ou, par exemple, génèrent une configuration après le déploiement. J'utilise principalement cette approche sur un serveur local pour offrir un outil de configuration automatique de la base de données, générer un schéma de base de données, etc.

Nous enregistrons les scripts nécessaires dans la section `scripts` de `composer.json` :

```json
"scripts": {
   "post-autoload-dump": "Baraja\\PackageManager\\PackageRegistrator::composerPostAutoloadDump"
}
```

Dans ce cas, la méthode statique `composerPostAutoloadDump` de la classe `Baraja\PackageManager\PackageRegistrator` est appelée automatiquement. Composer dispose de cette classe car il a d'abord effectué la génération des <a href="/autoloading-trid">classes d'autoloaders</a>.

Si nous voulons juste exécuter les scripts et ne pas effectuer d'actions inutiles avec Composer, la commande `composer dump` est très utile, car elle génère juste un nouvel autoloader (ce que je recommande pour le garder toujours à jour) et exécute ensuite les scripts immédiatement. Si vous voulez essayer d'utiliser des scripts, j'ai préparé un paquet prêt à l'emploi [Baraja PackageManager] (https://github.com/baraja-core/package-manager) qui implémente des scripts intelligents et une interface interactive pour le cadre Nette.

Versioning Vendor dans Git ?
------------------------

Une question dont nous discutons souvent avec les développeurs est de savoir s'il faut versionner le contenu du dossier `vendor` dans Git, ou le faire re-générer à chaque installation.

En général, la solution la plus propre semble être de **ne pas versionner** le contenu du tout et de tout installer à chaque fois. Mais la réalité est que, de temps en temps, un développeur arrête de développer un paquet, ou le supprime complètement. Le téléchargement constant de paquets complique également les installations et les mises à jour locales, ralentit les déploiements et peut parfois provoquer de brèves pannes de site lorsque de nouvelles versions de paquets sont téléchargées de manière incorrecte.

Je vois la suspension de Vendor comme une forme de "sécurité". Si les fichiers sont physiquement dans le système de gestion des versions, nous avons au moins une assurance rudimentaire sur le serveur de production que les paquets fonctionneront et que leur code est exactement le même que celui de l'installation locale. En outre, Vendor n'occupe souvent que des unités de Mo, ce qui vaut certainement la peine d'assurer un site fonctionnel compte tenu des capacités des disques d'aujourd'hui.

**Note pratique:** Pour le site moyen que je maintiens, `vendor` ne prend pas plus de `30 Mo`, ce qui est une capacité acceptable pour Git. Lorsque nous clonons un dépôt avec Junior, nous n'avons pas besoin de le former à la mise en service du site, qui fonctionne tout de suite.

Paquets Composer personnalisés
-----------------------

Vous pouvez créer vos propres paquets dans Composer, qu'ils soient disponibles publiquement (enregistrés sur [Packagist](https://packagist.org/)) ou privés (vous devez avoir votre propre serveur de paquets, par exemple [Satis](https://getcomposer.org/doc/articles/handling-private-packages.md)).

La question de la création, de la maintenance, du développement et du versioning des paquets est très complexe et fera l'objet d'un article séparé.

En attendant, vous pouvez lire l'article [Semantic Versioning] (https://semver.org/lang/cs/), que j'ai traduit pour vous.
