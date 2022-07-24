Comment optimiser efficacement la vitesse de chargement des pages
=================================================================

> id: '72f1d564-cb70-431c-a3dd-a3fdcaf14292'
> slug:
> 	- null
> 	fr: comment-optimiser-efficacement-la-vitesse-de-chargement-des-pages
> 
> cs: optimalizace-rychlosti-nacitani
> publicationDate: '2021-10-15 15:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '013c8f29c213d3f3f1df3fbe4be8d572'

Lorsque je voyage, je suis souvent confronté à des connexions Internet de mauvaise qualité, ce qui, en tant que concepteur de sites Web, m'amène à réfléchir aux principes de conception permettant de résoudre le problème de la vitesse des sites Web, même lorsque la connexion est mauvaise.

La pratique m'a permis de trouver quelques astuces utiles :

Les pages de destination importantes sont souvent simples et il est utile d'utiliser un style CSS personnalisé.
-----------------------------------------------------------------------------------

Par exemple, la page de connexion au CMS est souvent très simple. Un simple formulaire nécessite-t-il vraiment de télécharger l'ensemble de Bootstrap et d'autres frameworks CSS/JS ? Les pages importantes méritent d'être optimisées et de faire l'objet d'un code natif.

Une fois la page entièrement rendue, l'utilisateur dispose de quelques secondes pour remplir ses coordonnées, et nous pouvons télécharger et mettre en cache les styles restants dans le navigateur en arrière-plan. Au moment où l'utilisateur se connecte, il aura probablement déjà téléchargé Bootstrap (et s'il est sur Edge, il ne le fera pas...).

Au lieu des icônes, on peut souvent utiliser des emoji
-----------------------------------

Vraiment ! Les Emoji présentent de nombreux avantages pratiques :

- Elle n'occupe que 4 octets, ce qui la place au-dessus de toute image.
- Le téléchargement n'échoue jamais, et l'utilisateur voit toujours au moins une icône plutôt qu'un espace vide.
- S'affiche dans le style visuel de l'utilisateur
- Actuellement, il bénéficie déjà d'une large prise en charge des appareils
- Cela permet d'économiser la demande du serveur et de ne pas avoir à se préoccuper de l'endroit d'où provient l'image (cela peut être optimisé via un CDN, mais pourquoi, n'est-ce pas).
- Beaucoup d'icônes dont vous ne voulez probablement pas vous occuper. Par exemple, où trouver les icônes des drapeaux de tous les pays que vous souhaitez afficher dans l'administration ? Utilisez l'emoji : https://github.com/bara.../country/blob/main/flag-emoji.json

Utiliser les styles critiques directement dans le HTML lors du chargement du site
------------------------------------------------------

Je mets parfois les styles CSS importants définissant la mise en page et la mise en page de base directement dans le HTML. Je mets une limite de 1 KB, dans laquelle j'essaie de mettre le plus possible. L'inconvénient de cette approche est que les styles doivent être transférés à chaque demande et ne peuvent pas être mis en cache. En revanche, ils se téléchargent incomparablement plus vite qu'une image.

Je ne suis pas un expert en matière de vitesse de chargement, [Martin Michalek] (https://www.programia.cz/rozhovor-martin-michalek-rychlost-webu/) vous en dira plus.

Utilisez ajax !
--------------

J'utilise toujours l'ajax pour récupérer du contenu peu important ou lent. Cela ajoute un peu aux exigences technologiques de l'application, mais je peux me le permettre.

Un exemple d'un bon endroit pour utiliser ajax est presque tout dans l'administration. Très souvent, il y a beaucoup de données à récupérer, mais l'utilisateur n'est pas intéressé par tout. Lorsque l'utilisateur ne télécharge qu'un client javascript léger et que toutes les données sont récupérées via Vue et json, seule la quantité minimale de données est téléchargée et les réponses sont instantanées.

Comment faire cela dans Vue : https://vue.baraja.cz/api-a-ajax-ve-vue-js

En arrière-plan, j'utilise la bibliothèque pour Nette : https://github.com/baraja-core/structured-api.

Utilisez un CDN approprié
---------------------

Pour la distribution de contenu statique, je recommande d'utiliser un CDN pour tous les types de sites. Si vous ne pouvez pas mettre en place un CDN, utilisez au moins Cloudflare en mode proxy. Il mettra automatiquement en cache le contenu statique sur lui-même en fonction des en-têtes HTTP. Tous les fournisseurs d'hébergement n'ont pas une bonne configuration de Pops, et pour les longues routes, cela vous fera gagner des centaines de millisecondes à chaque requête.

Format d'image approprié
---------------------

L'un de mes juniors a récemment placé sur la page principale du site une image PNG qui contenait une photo et occupait 3 Mo. Il a dit que c'était OK parce que la page se chargeait rapidement sur sa connexion (c'est le cas sur son optique à la maison, n'est-ce pas...), et il a ajouté que nous transférons beaucoup de données de nos jours, comme la vidéo.

Je suis vieux jeu à ce sujet et j'optimise ce que je peux.

Photos en JPG, ou mieux WEBP. Mais sauvegarder une image au format JPG ne veut rien dire, vous devez la faire passer par un service d'optimisation : https://tinyjpg.com.

Si vous avez des images dans Git, cet outil peut les compresser automatiquement et envoyer une demande de pull : https://imgbot.net. Lorsqu'une nouvelle image est ajoutée, il envoie à nouveau le PR.

Si vous devez compresser des milliers d'images (comme toute une galerie de produits sur un site), j'utilise une application de bureau pour Mac pour le faire : https://imageoptim.com/mac.

Une compression appropriée des images permet généralement d'économiser le plus de données. Parfois même 50 %.
