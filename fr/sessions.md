Sessions - les cookies du serveur en PHP
========================================

> id: ab782ff0-d5ba-4115-9ebb-ab37978a6527
> slug:
> 	cs: sessions
> 	fr: sessions---les-cookies-du-serveur-en-php
> 
> publicationDate: '2019-11-06 17:06:18'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '3c84a09e5d180f6a47a58bea70b7ab21'

Nous avons souvent besoin de stocker plus d'informations dans <a href="/cookies">cookies</a>, mais la limite maximale pour les cookies est de 4 kB, ce qui est peu. Sessions résout ce problème en stockant les données sur le serveur web, et ne stocke qu'un court identifiant dans le navigateur du client pour savoir quelles données appartiennent à quel client.

Démarrer une session
---------------------

Avant de travailler sur les sessions, nous devons d'abord les démarrer. Cela se fait en appelant la fonction `session_start()` au début du script :

```php
session_start();
```

> Avertissement fort : aucune sortie vers le code HTML ne doit être exécutée avant d'appeler la fonction `session_start()` !

Sécurité de la session
-------------------

Le contenu de la session est stocké sur le serveur et seul l'identifiant est envoyé au navigateur du client. L'utilisateur n'a donc aucun moyen de savoir ce qui est stocké dans la session. La seule façon pour le script d'affecter l'utilisateur est de supprimer l'identifiant (auquel cas le script en génère un nouveau).

Récupération des données d'une session
----------------------

Toutes les sessions sont stockées dans la variable superglobale `$_SESSION` et peuvent être parcourues comme un tableau.

Par exemple, le nom de l'utilisateur actuellement connecté peut être récupéré en écrivant :

```php
echo $_SESSION['utilisateur'];
```

Note : La session peut ne pas toujours exister (par exemple, si vous êtes un nouvel utilisateur). Par conséquent, nous devrions toujours vérifier l'existence de ces messages avant toute inscription et proposer un autre message d'erreur si nécessaire.

```php
if (isset($_SESSION['utilisateur']) && $_SESSION['utilisateur']) {
    echo 'Utilisateur connecté :' . $_SESSION['utilisateur'];
} else {
    echo 'Personne ne s'est connecté.';
}
```

Sauvegarde des données dans la session
----------------------

La sauvegarde se fait comme un simple enregistrement de données dans une variable :

```php
$_SESSION['utilisateur'] = 'Honzik';
```

Le serveur web se chargera de la disposition technique du stockage correct sur le serveur et de l'envoi de l'identifiant à l'utilisateur.

Suppression de sessions
----------------

Nous pouvons supprimer les valeurs individuelles séparément en fonction de la clé :

```php
unset($_SESSION['utilisateur']);
```

Ou alternativement toutes les sessions disponibles :

```php
unset($_SESSION);
```

> Note : La suppression d'une session spécifique ne vide pas la valeur de la clé, mais supprime complètement la clé. Par conséquent, un message d'erreur sera émis si l'on tente de lire une clé inexistante. On peut toujours vérifier facilement l'existence d'une clé avec la fonction `isset()`.

Validité maximale de la session
---------------------------------

Chaque session enregistrée a une limite de durée de conservation sur le serveur. PHP contient directement un script cron qui supprime périodiquement les anciennes sessions.

La valeur par défaut est généralement `1440 secondes`, soit `24 minutes`.

L'augmentation de la valeur doit se faire à deux endroits :

- Dans <a href="/info">`php.ini`, on définit la longueur maximale de validité que le serveur conservera</a>. La valeur est définie par la directive `session.gc_maxlifetime`,
- Du côté du script PHP, vous devez spécifier la validité actuelle demandée.

Utilisation en PHP :

```php
// le serveur maintiendra la session jusqu'à 3600 secondes = 1 heure.
ini_set('session.gc_maxlifetime', '3600');

// tous les clients (navigateurs) seront
// session envoyée avec une validité d'exactement 3600 secondes
session_set_cookie_params(3600);

session_start(); // nous pouvons commencer la session !
```
