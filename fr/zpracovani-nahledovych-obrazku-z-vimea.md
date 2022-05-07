Traitement des vignettes et des méta-informations de Vimeo
==========================================================

> id: '8b8cbef0-210a-4883-be07-5004e8f43739'
> slug:
> 	cs: zpracovani-nahledovych-obrazku-z-vimea
> 	fr: traitement-des-vignettes-et-des-meta-informations-de-vimeo
> 
> publicationDate: '2020-09-19 17:32:31'
> mainCategoryId: a0143f3c-ac75-46dc-a514-d3c9417ded4e
> sourceContentHash: '93c29218f162fb5dc8d0f265968b0f8d'

Lorsque l'on intègre des vidéos de Vimeo dans une page (en tant qu'intégration HTML), on souhaite souvent obtenir également l'image et d'autres informations utiles telles que la durée de la vidéo, le titre complet, l'auteur, etc.

Heureusement, Vimeo fournit une API HTTP simple à partir de laquelle nous pouvons lire toutes les données basées sur le jeton vidéo.

Pour éviter d'avoir à écrire l'API vous-même, utilisez simplement [ready package](https://github.com/baraja-core/vimeo-video-api), qui intègre complètement l'API.

Vous installez le paquet avec la commande :

```shell
composer require baraja-core/vimeo-video-api
```

Il est facile à utiliser. Vous créez une instance du service `Baraja\VimeoAPI\VimeoVideoAPI` pour communiquer avec Vimeo conformément à la documentation, appelez la méthode `getInfo()`, passez le jeton vidéo et obtenez des informations détaillées sous la forme d'une entité `VideoInfo` à partir de laquelle toutes les informations disponibles peuvent être lues (toutes les informations ne sont pas toujours disponibles pour chaque vidéo).

De cette façon, vous pouvez interroger même les vidéos privées et celles qui ne sont pas accessibles au public. Mais vous devez toujours connaître leur jeton.

Liste de toutes les informations disponibles
---------

Une façon basique d'utiliser la bibliothèque ressemble à ceci :

```php
$api = new \Baraja\VimeoAPI\VimeoVideoAPI;

$token = 0; // Le jeton vidéo est un nombre entier
$info = $api->getInfo($token);

echo var_dump($info); // liste tout

// Imprimez la durée de la vidéo en secondes :
echo 'La durée de la vidéo est de :' . $info->getDuration();
```

La variable `$info` stocke toutes les informations descriptives d'une vidéo particulière. Un aperçu de toutes les méthodes disponibles [se trouve dans l'implémentation] (https://github.com/baraja-core/vimeo-video-api/blob/master/src/VideoInfo.php).
