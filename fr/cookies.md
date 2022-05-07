Cookies en PHP
==============

> id: '392dd88b-d2f5-4943-a993-01aaad7ccd32'
> slug:
> 	cs: cookies
> 	fr: cookies-en-php
> 
> perex: 'Cookies je krátká textová informace v paměti prohlížeče, kam můžeme v PHP zapisovat a označit si uživatele.'
> publicationDate: '2019-09-11 10:18:29'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '78f9aad0278acd51b98179e7d29fe3d8'

**Avertissement : ** Cet article a été écrit il y a plusieurs années et certaines informations peuvent être dépassées ou incorrectes. Veuillez garder cela à l'esprit lors de votre lecture.

Les cookies sont de petits éléments d'information textuels stockés dans le navigateur d'un visiteur de site web. Ils sont toujours transférés avec chaque page rechargée, et peuvent être supprimés, modifiés et lus par l'utilisateur à tout moment, ils ne sont donc pas bien adaptés au stockage d'informations personnelles.

*Avertissement : si votre site web utilise des cookies pour suivre les utilisateurs ou des modules complémentaires de tiers (par exemple, le bouton Facebook like, le compteur de trafic de Google Analytics, les bannières publicitaires), vous devez en informer l'utilisateur.

> "Une remarque supplémentaire : votre site ne doit pas contenir de codes de publicité ou de mesure tant que vous n'avez pas obtenu le consentement. Et ça craint."
>
> -- <a href="https://phpfashion.com/jak-na-souhlas-s-cookie-ve-zkurvene-eu">David Grudl</a>

Lire une valeur à partir d'un cookie
--------------------------

Tous les cookies sont stockés dans la variable superglobale `$_COOKIE`, qui stocke chaque clé comme un tableau.

Par exemple, si nous avons stocké le nom de l'utilisateur actuellement connecté sous la clé `user` dans le cookie, nous pouvons le retrouver facilement :

```php
echo $_COOKIE['utilisateur'];
```

Veuillez noter que les cookies peuvent ne pas toujours exister (par exemple, si vous êtes un nouvel utilisateur). Il convient donc de toujours vérifier l'existence de cookies avant toute inscription et de proposer un message d'erreur alternatif si nécessaire.

```php
if (isset($_COOKIE['utilisateur']) && $_COOKIE['utilisateur']) {
    echo 'Utilisateur connecté :' . $_COOKIE['utilisateur'];
} else {
    echo 'Personne ne s'est connecté.';
}
```

Obtenir tous les cookies disponibles
--------------------------------

Comme tous les cookies sont stockés dans la variable superglobale `$_COOKIE`, ils peuvent être facilement listés :

```php
var_dump($_COOKIE);
```

Ou alternativement, parcourir le cycle et obtenir toutes les clés et valeurs :

```php
foreach($_COOKIE as $key => $value) {
    echo $key . ':' . $value;	// écrire la clé et la valeur
    echo '<br>';				// enrouler la ligne
}
```

Stockage de la valeur dans un cookie
--------------------------

La fonction `setcookie()` est utilisée pour stocker des données dans des cookies.

Le premier paramètre est la clé du cookie, qui est utilisée pour le lire à partir du champ `$_COOKIE`, et le second paramètre est la donnée elle-même sous forme de chaîne.

Avec le troisième paramètre, nous pouvons (facultativement) définir la période de validité pour laquelle le cookie sera disponible. Le temps de disponibilité est donné comme <a href="/date">timestamp</a>, donc si nous voulons définir un cookie avec une validité de 1 heure à partir de ce moment, nous devons juste écrire `time() + 3600`.

```php
$data = 'Un contenu que nous voulons stocker.';

setcookie('TestCookie', $data);
setcookie('TestCookie', $data, time() + 3600);
```

Stockage de données plus importantes
-------------------

Les cookies ne sont pas adaptés au stockage de données plus importantes (les navigateurs n'autorisent généralement que 4 kB et un maximum de 20 cookies à être stockés, la taille comprenant également les noms des cookies, les paramètres de validité, etc.)

Il est préférable de stocker des données plus importantes sur le serveur et de mettre simplement un identifiant dans le cookie, grâce auquel nous pouvons savoir à quel utilisateur il appartient. Cette méthode est appelée `$_SESSION` et est discutée dans un article séparé.

Si vous n'avez pas nécessairement besoin de stocker des données toujours synchrones sur le serveur, vous pouvez utiliser le stockage **<a href="https://jecas.cz/localstorage">localstorage</a>** disponible en javascript. Sa capacité est de l'ordre de quelques Mo et les données ne sont pas soumises à expiration.
