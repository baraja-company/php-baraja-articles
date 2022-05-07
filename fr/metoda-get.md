Récupération des paramètres de l'URL par la méthode GET
=======================================================

> id: bbf2cb2c-f7f7-4be9-a9cd-960014db0f51
> slug:
> 	cs: metoda-get
> 	fr: recuperation-des-parametres-de-l-url-par-la-methode-get
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: caebe20b6b7cdf0b3703ff55dad6f094

Vous savez, vous avez une page ouverte, vous suivez l'URL et vous voyez un point d'interrogation avec quelques paramètres. Un programmeur inexpérimenté penserait qu'il s'agit de fichiers séparés, mais voilà... Essayez de créer un fichier dont le nom comporte un point d'interrogation (cela ne fonctionne pas). **C'est la raison pour laquelle cet article a été écrit**.

Qu'est-ce que c'est ?
--------------------------

En fait, il s'agit d'un fichier unique auquel vous transmettez des variables via une URL. J'ai donc, disons, un fichier **index.php**, et je lui transmets le nom de l'article : **index.php?clanek=o-php**.

Code + explication
--------------------------

La variable superglobale `$_GET` contient des clés avec des paramètres de l'URL

```php
echo $_GET['Article'] ?? '';
```

Sécurité et limites de longueur
--------------------------

La méthode GET n'est pas sécurisée, les données confidentielles ne doivent donc pas être envoyées par ce biais. L'une des principales raisons est qu'il s'agit d'une communication non cryptée et qu'elle est stockée dans l'historique.

Les données confidentielles ou tout simplement tout doivent être envoyées en utilisant la méthode <a href="/method-post">POST</a>. GET est plus adapté aux furmulaires où il est bon de montrer des paramètres (comme les moteurs de recherche, la page de l'article) afin que la page puisse être liée.

La durée du GET n'est pas illimitée ! Beaucoup de débutants paient pour cela. La longueur maximale est d'environ 1024 caractères (certains endroits disent 1088). Ainsi, pour les textes plus longs, envoyez <a href="/method-post">POST</a> avec.
