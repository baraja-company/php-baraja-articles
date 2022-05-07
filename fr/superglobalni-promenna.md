Variables superglobales
=======================

> id: bc2107ee-6551-4559-8c02-9cecdf98d33b
> slug:
> 	cs: superglobalni-promenna
> 	fr: variables-superglobales
> 
> publicationDate: '2019-11-01 09:29:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '792e217f1331f38ed0719d7abeb05e67'

Les variables superglobales sont utilisées pour transmettre l'état global de l'application et la communication HTTP.

Le principal avantage de ces variables est qu'elles sont toujours et partout disponibles. En pratique, il s'agit de tableaux de valeurs où l'on accède à des informations spécifiques par index. Dans différents contextes, la disponibilité des clés peut varier (expliqué ci-dessous).

Types de variables superglobales
--------------------------------

Tous les superglobals en PHP sont des tableaux et sont désignés par un signe dollar suivi d'un underscore (sauf `$GLOBALS`) et de caractères majuscules.

Dans `PHP 7`, il y a notamment les éléments suivants :

| Variable | Description |
|-------------|-------|
| `$_GET` | Paramètres URL <a href="/methods-odesilani-dat">envoyés par la méthode GET</a>
| `$_POST` | Données du formulaire <a href="/methods-odesilani-dat">envoyé par POST</a>. Notez que <a href="/ajax-post">peut se comporter différemment en ajax</a>.
| `$_REQUEST` | Données de formulaire envoyées par une méthode quelconque (`$_GET`, `$_POST` et `$_REQUEST`).
| `$_FILES` | Informations techniques sur les fichiers actuellement téléchargés, par exemple via la construction `<input type="file">`.
| `$_SERVER` | <a href="/info">Paramètres du serveur web</a>, adresse IP, configuration... elle varie en fonction de l'environnement (lors de l'appel d'un script PHP depuis Terminal, elle contiendra des valeurs différentes et par exemple les informations sur la requête en cours seront manquantes).
| `$_COOKIE` | Configuré <a href="/cookies">cookies</a>.
| `$_SESSION` | Données de la session (<a href="/sessions">session</a>), si elle existe et a été définie dans le passé.
| `$GLOBALS` | **Attention, il ne contient pas de trait de soulignement dans le nom!** C'est ce qu'on appelle <a href="global-variable">global-variable</a> et une notation alternative pour le mot-clé `global`. Si vous avez une variable globale `$variable` dans votre application, vous pouvez également y accéder avec la construction `$GLOBALS["variable"]`. Cependant, l'utilisation de variables globales est une solution mauvaise et impure par conception, il vaut donc mieux ne pas la faire.
| `$_ENV` | Informations sur l'environnement actuel où PHP est exécuté.

Il est facile de répertorier toutes les valeurs existantes :

```php
foreach ($_SERVER as $key => $value {
	echo $key . ':' . $value . '<br>';
}
```

> Note : Tous les index ne doivent pas toujours exister (par exemple, si le script exécute cron en mode CLI, l'index avec l'URL de la page ou l'adresse IP de la requête n'existera pas).

Accès aux variables
-------------------

Je recommande que toutes les variables globales (sauf `$_SESSION`) soient en lecture seule. En effet, ils contiennent des données d'application globales et d'autres codes peuvent en tenir compte (par exemple, une autre bibliothèque installée).

Un autre inconvénient de l'état global est que vous ne pouvez pas toujours compter sur des valeurs exactes, même si elles existent, donc vous devriez toujours vérifier leurs clés avec la construction `isset()`.

Pour enregistrer un nouveau cookie, utilisez `setcookie()` et n'insérez pas la valeur directement. Cela est dû au fait qu'il est en lecture seule.

Les leçons apprises
-------

Ne faites jamais aveuglément confiance aux valeurs des variables superglobales !

L'utilisateur peut utiliser l'URL et les en-têtes envoyés pour influencer la manière dont les valeurs sont définies. Toutes les entrées doivent toujours être soigneusement validées.

Les globaux de registre - le problème avec l'ancienne version de PHP
------------------------------------------

Dans l'ancienne version de PHP (jusqu'à `5.4.0`), il y avait une directive spéciale `register-globals` (configurable dans `php.ini`) qui faisait que tous les paramètres passés dans une URL étaient automatiquement enregistrés comme variables.

Par exemple :

Un utilisateur est arrivé à l'URL : `https://example.com/script.php?var=24`.

Et PHP a automatiquement créé une variable `$var` avec la valeur `24` dans le script.

Cela a donc fonctionné de manière classique :

```php
echo $var;
```

N'importe qui pourrait donc glisser n'importe quelle variable dans le script et en modifier le contenu. De toute évidence, la sécurité n'a pas toujours été une priorité. Pas vraiment.

Autres sources
------------

Pour une description plus détaillée, voir le <a href="https://www.php.net/manual/en/language.variables.superglobals.php">manuel officiel</a>.
