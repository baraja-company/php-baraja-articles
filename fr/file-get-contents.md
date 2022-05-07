Fichier_get_contents
====================

> id: '6c8889f1-95e7-4540-9c3a-0225c6383954'
> slug:
> 	cs: file-get-contents
> 	fr: fichier-get-contents
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: f8b180a5f7208dab0d37761a5acdc9e3

La fonction **file_get_contents** est utilisée pour lire un fichier et mettre son contenu dans une variable. Cette fonction est similaire à la fonction <a href="/include">include</a>, mais contrairement à include, elle peut récupérer des fichiers distants sur Internet et transférer leur contenu via des variables.

Echantillon
------

L'une ou l'autre de ces fonctions peut être utilisée pour charger un fichier local à partir du disque :

```php
$news = file_get_contents('news.html');

echo 'Dernières nouvelles:<br>' . $news;
```

Ou à partir d'une URL distante :

```php
$page = file_get_contents('https://www.google.com');

echo $page;
```

Lors de la récupération d'une URL, toute adresse peut être téléchargée et son contenu peut être récupéré sous forme de chaîne dans une variable. Dans le cas du HTML, il s'agit du code source.

La page est rendue de manière incorrecte
----------------------------

En effet, le code HTML est transmis exactement comme il est placé sur l'URL.

Si le chemin d'accès à l'image est par exemple `<img src="kocka.png">`, il se peut que ce fichier n'existe pas dans le contexte de notre serveur, nous devons donc corriger le chemin d'accès pour qu'il soit par exemple : `<img src="https://server.cz/kocka.png">`.
