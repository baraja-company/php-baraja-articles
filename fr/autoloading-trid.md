Classes à chargement automatique en PHP
=======================================

> id: f6cd5762-261f-4153-b27b-075dd8b5ed13
> slug:
> 	cs: autoloading-trid
> 	fr: classes-a-chargement-automatique-en-php
> 
> publicationDate: '2020-02-09 10:00:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '441a1d4107bb32e8cfe4dbb926c2decd'

Je suis sûr que vous le savez, lorsque vous programmez des scripts PHP, nous divisons le code en plusieurs fichiers et pour avoir toutes les parties disponibles, nous les chargeons avec une série d'appels `include`, `require` ou de préférence `require_once`, qui garantit un chargement unique.

Dans le code, cela ressemble à ceci :

```php
require_once 'Routeur.php';
require_once 'Page.php';
require_once 'Paginator.php';
```

Le principal inconvénient de cette approche est que le programmeur doit constamment s'assurer que tout est toujours chargé. Mais s'il charge beaucoup, il perd inutilement de la performance et atteint le disque plusieurs fois. La solution manuelle n'a donc que des problèmes.

Chargement automatique natif
-------------------

Heureusement, il y a un support natif en PHP pour ce qu'on appelle le **Class Autoloading**, qui est une logique dans le code qui charge un fichier de classe seulement quand il est nécessaire (typiquement quand une instance est créée).

Une mise en œuvre simple pourrait alors ressembler à ceci :

```php
spl_autoload_register(function (string $className): void {
    include 'src/' . $className . '.php';
});

$obj  = new MyClass1();
$obj2 = new MyClass2();
```

Lors de la création d'une instance de la classe `MyClass1`, la fonction `spl_autoload_register` lit le fichier `MyClass1.php` du répertoire `src`. Cette implémentation suppose que chaque classe se trouve dans un fichier séparé appelé par le nom de la classe ou de l'interface.

Cas plus complexes d'autoloading
-------------------------------

Dans une application réelle, de nombreuses situations désagréables peuvent survenir et compliquer le chargement automatique, par exemple :

- La classe ou l'interface n'existe pas du tout
- Le fichier a déjà été chargé une fois
- Il y a plusieurs classes ou interfaces dans le même fichier
- Le chemin d'accès à la classe ou à l'interface ne correspond pas au nom.
- L'emplacement de la classe ou de l'interface change au fil du temps

Cependant, nous n'avons pas besoin de programmer notre propre solution pour tout cela, mais pouvons utiliser la solution existante une fois conçue.

Si vous utilisez <a href="https://getcomposer.org/doc/01-basic-usage.md">Composer</a>, vous utilisez probablement aussi son autoloading natif. En effet, lorsque vous installez un paquet, Composer génère automatiquement une "carte des classes", qui est un aperçu des classes et de leur emplacement physique.

Puis au début du code (typiquement dans `index.php`) vous utilisez simplement :

```php
require __DIR__ . '/vendor/autoload.php';
```

Cependant, l'autoloading n'est généré qu'une seule fois lorsque la commande `composer dump` est appelée, il est donc nécessaire de régénérer l'autoloading à chaque fois que l'application change.

RobotLoader - une solution élégante pour le développement
----------------------------------------

Pour le développement local, j'aime beaucoup le paquet `nette/robot-loader`, qui parcourt automatiquement la structure des répertoires et met en cache les emplacements des classes. Ainsi, si nous chargeons une classe, il regarde d'abord dans le cache et seulement si elle n'existe pas, il réindexe automatiquement le projet. Par conséquent, le programmeur n'a pas besoin de savoir où se trouve un fichier ou une classe et se contente de programmer.

Installation via Composer :

```txt
composer require nette/robot-loader
```

L'explication de base de la fonctionnalité est décrite dans la <a href="https://doc.nette.org/cs/3.0/robotloader">documentation</a> elle-même :

> De la même manière que le robot de Google parcourt et indexe les pages Web, RobotLoader parcourt tous les scripts PHP et note les classes, interfaces et traits qu'il y trouve. Il met ensuite en cache les résultats de ses recherches et les utilise lors de la prochaine requête. Il vous suffit donc de préciser les répertoires à explorer et ceux à mettre en cache.

Il est ensuite extrêmement facile à utiliser :

```php
$loader = new Nette\Loaders\RobotLoader;

// ajouter les répertoires que RobotLoader doit indexer
$loader->addDirectory(__DIR__ . '/app');
$loader->addDirectory(__DIR__ . '/libs');

// définir la mise en cache sur le disque dans le répertoire 'temp'.
$loader->setTempDirectory(__DIR__ . '/temp');
$loader->register(); // lancement du RobotLoader
```

Le réglage de `$loader->setAutoRefresh(true or false)` détermine si RobotLoader doit réindexer les fichiers lorsqu'il rencontre une nouvelle classe. Cette option doit être désactivée sur les serveurs de production.

Voilà, maintenant vous n'aurez plus jamais à vous occuper de l'autoloading.

Solution combinée
------------------

J'utilise une solution combinée lorsque je développe un projet réel.

La façon dont cela fonctionne dans la vie réelle est que je charge les paquets installés via Composer autoload (qui est très efficace) et cela résout le chargement de toutes les classes dans le répertoire `vendor`.

Le code pour le projet spécifique est alors placé dans le répertoire `app`, où je gère l'autoloading de quelques classes seulement via RobotLoader. L'important est de toujours garder l'application concrète aussi petite que possible et d'utiliser autant que possible des paquets prêts à l'emploi, ce qui favorise grandement la réutilisation.

La structure du projet se présente comme suit :

```txt
/app
    Bootstrap.php <-- konfigurace
    /model
        UserForm.php <-- projektové třídy
        RegisterFactory.php
        ...
/vendor
    ... <-- knihovny
/www
    index.php <-- inicializace
```

Test d'autoloading
------------------------

Il peut arriver que tous les fichiers ne se chargent pas toujours et que vous rencontriez des problèmes.

Pour le débogage, je recommande la fonction <a href="/get-list-of-all-loaded-files">get_included_files()</a>.
