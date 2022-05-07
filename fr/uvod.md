Introduction à PHP
==================

> id: '66b7fd22-6195-4999-ad8d-b799afdc1876'
> slug:
> 	cs: uvod
> 	fr: introduction-a-php
> 
> perex: 'Úvodní tutoriál do jazyka PHP, možnosti jazyka, informace o tvorbě webu v PHP.'
> publicationDate: '2019-09-29 19:25:06'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: db6ba8fe6d5ca28aab62d7302c1d7527

Il s'agit d'un cours en ligne pour apprendre le PHP, conçu pour l'enseignement général. Il est disponible gratuitement sur ce site. Les textes ne doivent pas être utilisés dans un cours payant sans confirmation écrite. Vous pouvez utiliser les échantillons utilisés sur le site dans votre application sans autre restriction. [Conditions générales] (https://baraja.cz/vseobecne-obchodni-podminky).

Mise à niveau de HTML
--------------

"Mise à niveau" ? Cela ressemble davantage à une manipulation médiatique, et c'est effectivement le cas.

Il n'y a pas de mise à niveau. Le PHP conserve toutes les fonctionnalités et caractéristiques d'un document HTML pur, il ajoute simplement de nouvelles options d'écriture et de déploiement. Super, non ?

Vous savez déjà, pour avoir développé des pages HTML statiques, que le code est constitué de balises, qui sont définies comme des mots-clés entourés de crochets pointus (par exemple, `<b>Hello!</b>` exprime le texte en gras **Hello!**).

PHP est inséré dans la page HTML sous la forme des balises `<?php` et `?>`, à l'intérieur desquelles d'autres logiques d'application sont écrites. Il est important de noter que PHP a sa propre syntaxe (les règles selon lesquelles le code est écrit) et, contrairement à HTML, n'est pas tolérant aux erreurs.

Des exemples spécifiques de la manière de procéder et de la signification de chaque balise seront donnés ultérieurement. Pour commencer, il est important de comprendre les principes généraux du fonctionnement de PHP sur le serveur et la façon dont le code est traité.

Le flux de communication entre l'utilisateur et le serveur.
---------------------------------------

Pour une page HTML ordinaire, voici en gros comment cela fonctionne :

> L'utilisateur envoie une requête (demande une page HTML spécifique), le serveur regarde le disque et renvoie exactement ce qui est stocké. Il ne se passe rien de spécial, et n'attendez rien de plus. Les pages seront simplement statiques, sans possibilité d'interaction avec le serveur.

Si nous ajoutons le PHP, cependant, des merveilles vont commencer à se produire :

> L'utilisateur demande à nouveau la page. Le serveur ouvre le fichier sur le disque, mais constate qu'il ne contient pas seulement du HTML pur, mais aussi des balises spéciales qui indiquent un script PHP. Il les évalue donc d'abord et envoie ensuite ce que PHP a généré.

L'évaluation du code PHP est effectuée par défaut à chaque fois que la page est chargée, dans le futur vous apprendrez comment mettre le code en cache (le stocker compilé pour un traitement plus rapide).

La différence entre le traitement des scripts PHP et C/C++
------------------------------------------

Vous avez peut-être appris à utiliser le langage `C` ou `C++` à l'école. PHP est directement basé sur la syntaxe du langage `C`, et à l'intérieur du noyau le langage `C` est utilisé, il est donc bon de connaître les points communs et inversement les différences.

Le principe de base des programmes compilés courants (qui s'exécutent localement directement sur votre ordinateur ou votre smartphone) consiste à charger la logique applicative dans la mémoire d'exploitation, qui communique directement avec le système d'exploitation, lequel reçoit les entrées de l'utilisateur et affiche ensuite les résultats du programme. L'important est que le programme fonctionne de manière isolée du début à la fin.

PHP démarre à chaque requête pour rendre une page, recharge tout le code et les données à chaque fois, puis sort. Les scripts PHP ont donc une durée de vie littéralement yuppie et n'existent généralement que pendant quelques dizaines de millisecondes.

L'avantage de cette approche est un niveau d'isolation plus élevé : si quelque chose ne va pas, tout est rechargé lors du prochain chargement de la page. D'un autre côté, cette approche a des exigences de performance plus élevées car nous devons nous connecter à la base de données, lire les fichiers sur le disque, etc. de manière répétée, par exemple.

Dans le futur, vous apprendrez que vous pouvez garder les scripts PHP chargés dans la mémoire d'exploitation en utilisant l'extension `OP Cache`, que la plupart des nouveaux serveurs (à partir de PHP 7.1) ont mis dans la configuration de base.

Téléchargement de scripts PHP étrangers
--------------------------

Une question relativement courante dont nous discutons avec les étudiants est de savoir comment télécharger des scripts PHP étrangers depuis le serveur et examiner leur code source. Cette question est précédée par la considération que le code HTML d'une page peut être facilement affiché dans un navigateur web.

La réponse est que **les scripts PHP ne peuvent pas être téléchargés**. En effet, le code PHP est d'abord évalué sur le serveur web, puis le code HTML généré (ou toute autre sortie) est envoyé au navigateur de l'utilisateur. Par conséquent, seule la sortie du script PHP peut être téléchargée, et non le script lui-même.

Comment fonctionne la génération vers le HTML ?
---------------------------------

Cela ne fonctionne pas littéralement comme ça, vous allez surfer sur des pages HTML. Le script PHP doit toujours se trouver dans un fichier PHP.

Jusqu'à présent, la plupart d'entre vous étaient habitués à créer d'énormes dossiers remplis de fichiers se terminant par l'extension **.html**. Maintenant, il y aura beaucoup moins de fichiers. Dans le cas extrême, il peut s'agir d'un seul fichier.
