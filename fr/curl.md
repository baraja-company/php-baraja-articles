cURL en PHP - téléchargement de données via une URL
===================================================

> id: '0bd1aed6-460d-4b63-9afe-f5087d1c6046'
> slug:
> 	cs: curl
> 	fr: curl-en-php---telechargement-de-donnees-via-une-url
> 
> perex: Knihovna cURL je robustní PHP knihovna pro pokročilou HTTP komunikaci.
> publicationDate: '2020-02-15 22:14:32'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '7c565449293cf1ce4950f309abaf04d0'

La bibliothèque PHP `cURL` est un bon moyen de télécharger des données depuis un serveur étranger.

Sur la base d'une requête, il construit une demande HTTP qu'il envoie au serveur cible, et une fois téléchargé, il contient une API pour une manipulation (relativement) facile des données.

Contrairement à la fonction native `file_get_contents` (par laquelle nous pouvons aussi faire des requêtes HTTP), il offre de bien meilleures options de configuration et télécharge les pages/fichiers comme un vrai navigateur.

> La fonction `file_get_contents` utilise la bibliothèque `cURL` en interne, elle n'a simplement pas d'options de configuration aussi détaillées.

Détection du mode cURL dans une requête
----------------------------

Il est souvent utile de détecter si la requête courante a été faite via `cUrl` ou classiquement dans le navigateur.

Il n'y a pas d'implémentation directe pour cela en PHP, mais nous pouvons écrire une fonction simple nous-mêmes :

```php
function isCurl(): bool
{
    return str_contains($_SERVER['HTTP_USER_AGENT'] ?? '', 'bouclette');
}
```

Si vous avez Linux et son Terminal, ou si vous êtes sur un Mac, essayez cette commande :

```shell
curl https://php.baraja.cz/curl
```

La commande effectue une requête interne à ce site et renvoie le résultat.

Si l'application n'a pas détecté de requête cURL, le HTML sera renvoyé comme si la requête provenait du navigateur. Cependant, puisque les types de demande sont détectés, rien ne nous empêche de renvoyer un article Markdown nettoyé.

L'avantage est alors de disposer de données bien mieux nettoyées. Nous montrons le HTML formaté à l'utilisateur dans le navigateur, mais seulement le contenu de base au robot.

Utilisation détaillée de l'API en PHP
--------------------------

Si vous cherchez une utilisation détaillée de cUrl, je vous recommande de lire la <a href="https://www.php.net/manual/en/book.curl.php">documentation officielle</a>, qui est toujours à jour.

Pour une utilisation occasionnelle, il existe une <a href="https://docs.guzzlephp.org/en/stable/">**Guzzle**</a> bibliothèque qui gère
