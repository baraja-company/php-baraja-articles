Hachage localement sensible
===========================

> id: '76e09294-08ea-4dde-a431-2116595d9f04'
> slug:
> 	cs: lokalne-senzitivni-hashovani
> 	fr: hachage-localement-sensible
> 
> publicationDate: '2022-07-11 10:45:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: c23def9586aa05d73c4f1cf1fd1dda43

Le principe de la plupart des fonctions de hachage d'empreintes de documents est qu'elles renvoient toujours la même sortie pour chaque entrée. C'est ce qu'on appelle un comportement déterministe. En même temps, un petit changement dans l'entrée provoquera un grand changement dans la sortie (renvoyant effectivement un hachage complètement différent). C'est particulièrement utile lorsque nous voulons vérifier si le document d'entrée a changé, et si c'est le cas, nous obtenons quelque chose de complètement différent. Un exemple d'une telle fonction est MD5 ou SHA1.

Si nous voulons déduire le contenu de l'entrée originale à partir du hachage, il n'y a pas de moyen facile de le faire et nous n'avons pas d'autre choix que d'utiliser la force brute pour chaque option jusqu'à ce que nous obtenions un résultat. En effet, en raison de l'importante variation de la sortie, nous ne pouvons pas déterminer par itérations si nous nous rapprochons de la cible ou non.

Certaines fonctions de hachage, telles que BCrypt, pour la même entrée, renverront une sortie différente à chaque fois, ce qui les rend robustes au hachage de mots de passe. En fait, la sortie contient directement le sel, ce qui rend impossible une attaque dite par dictionnaire. Cette fonction est utile uniquement pour le hachage de mots de passe, mais elle est très peu adaptée à l'examen de documents.

Contrôle de la similarité des documents et recherche de doublons dans le contenu
-----------------------------------------------------------

Imaginons que nous soyons en train de résoudre l'algorithme d'un moteur de recherche web qui veut vérifier dans quelle mesure une page web a changé. Ou encore, il veut vérifier rapidement si le texte d'une page est similaire à celui d'une autre page, voire complètement dupliqué.

Une façon de le vérifier est de comparer chaque page entre elles, ce qui est très gourmand en ressources système. Une autre option consiste à calculer un hachage SHA1, mais il s'agit du contenu de l'ensemble du document, et si la page change d'un seul caractère, nous obtenons un hachage différent - il ne convient donc pas à ces fins.

Par conséquent, une solution possible consiste à calculer le hachage de différentes zones du document de manière différente, puis à rechercher les changements dans la sortie de la fonction de hachage.

Le principe du hachage local-sensible
----------------------------------

Nous avons téléchargé le code HTML d'une page web pour laquelle nous voulons calculer un hash. Nous divisons le document en différentes régions (cellules) par un algorithme qui doit être configuré correctement.

Nous hachons chaque région indépendamment en utilisant un algorithme commun et concaténons le résultat en une seule chaîne.

La sortie peut alors être, par exemple, `3791744029724361934` (ce hachage de contenu a été fourni par l'outil Ahrefs).

Par exemple, si nous savons que le contenu de l'en-tête représente les 5 premiers caractères et que le pied de page représente les 5 derniers caractères, nous savons que le milieu de la chaîne représente le contenu de la page. Si le contenu du pied de page change sur toutes les pages (par exemple, le webmaster ajoute un nouveau lien, la date de mise à jour change, etc.), alors lorsque la nouvelle version de la page est téléchargée, seuls quelques caractères du hachage dans la zone de droite changent légèrement, et nous savons que seul le pied de page a changé, mais que le contenu reste inchangé. Nous pouvons donc ignorer un tel changement et ne pas avoir à parcourir l'ensemble du site.

Comment Google optimise-t-il l'exploration du Web ?
----------------------------------------

Les robots Internet doivent économiser là où ils le peuvent. L'exploration du Web est très coûteuse et les mises à jour coûtent du temps de calcul.

Par exemple, en observant le comportement de l'algorithme du robot de Google, j'ai constaté qu'il ne réagit qu'aux grands changements de contenu. Si la page change peu, elle est explorée de façon normale. Mais lorsque, par exemple, le pied de page et l'en-tête d'un site changent de manière significative, il l'évalue comme une refonte et passe en revue la majeure partie du site en une seule fois pour obtenir le nouveau look le plus rapidement possible.

Il détecte également les doublons entre les sites de manière similaire. Lorsqu'un groupe de pages est très similaire ou a le même contenu, il obtient un hachage très similaire, que le robot peut utiliser pour vérifier rapidement que les documents sont similaires et peut sélectionner le document canonique et ignorer le reste.

Mise en œuvre de la fonction de hachage
-----------------------------

Il n'existe pas d'implémentation prête à l'emploi de la fonction de hachage locale en PHP. En même temps, je ne connais pas de paquetage disponible librement qui implémente bien cette fonction.
