Forme pluriannuelle
===================

> id: '2abacddd-8a6b-4d25-a387-603fc7abc333'
> slug:
> 	cs: vicekrokovy-formular
> 	fr: forme-pluriannuelle
> 
> publicationDate: '2019-09-16 09:30:19'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '69cb85c856478d588b96e9db9e695d97'

Parfois, nous devons diviser le formulaire en plusieurs parties (pages), les traiter séparément, puis les assembler en un seul résultat.

Cet article décrit les méthodes et les modèles de conception pour y parvenir.

> **Note:**
>
> Le problème de la division d'un formulaire en plusieurs étapes est très complexe, surtout si l'on veut le faire correctement. J'ai rencontré de nombreuses approches au cours de ma vie, dont je vais parler ici. Certaines approches semblent séduisantes, mais sont naïves et ne fonctionnent que dans des cas spécifiques. Pour chaque approche, je décris quand elle a du sens et quelles approches n'en ont pas.

Conception d'une solution théorique
-------------------------

En général, l'objectif est de récupérer les données de base du premier formulaire sur la première page, de les valider, puis de les enregistrer "quelque part" et d'afficher la page suivante.

Lorsque l'utilisateur arrive à la dernière page, le formulaire global doit être soumis et les entrées traitées.

À chaque étape, il est important de toujours valider soigneusement toutes les données et de permettre à l'utilisateur de passer d'une page à l'autre à volonté afin qu'il puisse corriger les données en cas d'erreur. En outre, si le formulaire doit être rendu de manière conditionnelle en fonction des données déjà obtenues, il s'agit d'un processus très exigeant.

Mise en œuvre des formulaires eux-mêmes
--------------------------------

Nous pouvons soit implémenter nous-mêmes les différents formulaires en HTML pur et gérer ensuite le traitement en PHP, soit utiliser des solutions prêtes à l'emploi telles que <a href="https://doc.nette.org/cs/3.0/forms">Nette forms</a>.

> **Exemple tiré de la vie:**
>
> Très souvent, des programmeurs débutants m'envoient des courriels pour me poser des questions apparemment simples pour lesquelles il existe une solution toute faite. Par exemple, spécifiquement sur le traitement des formulaires en PHP.
>
> Je recommande toujours d'éviter tout traitement manuel et d'utiliser une solution toute prête à la place. En réalité, il est très compliqué d'implémenter correctement, par exemple, la validation de l'email saisi et que 2 mots de passe dans 2 champs correspondent, alors que nous voulons rediriger l'utilisateur vers le formulaire pré-rempli en fonction de ses données et avec des messages d'erreur en cas d'erreur.
>
> Parce que les gens **ne savent pas qu'ils ne savent pas qu'ils ne savent pas** et qu'au lieu d'investir une heure de temps dans l'apprentissage d'une solution toute faite pour 99,99 % des problèmes, ils préfèrent choisir leur propre solution, qu'ils passent des dizaines d'heures à déboguer, et il y a encore des cas où les formulaires ne fonctionnent pas, produisent des erreurs, présentent des failles de sécurité et ne protègent pas les données saisies.

Le but de cette étape est donc de mettre en place plusieurs pages à différentes URL qui contiendront des formulaires vierges.

Je recommande d'implémenter chaque formulaire indépendamment des autres (de manière atomique) et de gérer le passage d'état sur une couche d'application différente. La raison en est que, dans la pratique, chaque formulaire traitera la validation des données différemment, écrira ses sorties différemment, traitera les erreurs différemment, et nous voudrons probablement l'étendre ou le modifier au fil du temps, de sorte que nous n'avons pas besoin de connaître le contexte de l'ensemble du processus et de modifier des dizaines de sites pour le faire.

Transfert d'État
---------------

Lors du traitement du premier formulaire, nous voulons d'abord valider les données reçues et, si elles sont correctes, rediriger l'utilisateur vers la deuxième étape. C'est une bonne idée de traiter la redirection comme une redirection HTTP, car il peut facilement arriver que les données ne soient pas valides, auquel cas nous voulons renvoyer l'utilisateur au premier formulaire et non à l'étape suivante.

Nous pouvons essentiellement stocker les états de 4 façons :

**Non recommandé:**

- Cette solution présente l'inconvénient que l'utilisateur peut modifier une fois les données déjà envoyées dans l'URL et ainsi falsifier l'entrée. Nous pouvons également divulguer des informations sensibles telles que des mots de passe dans l'URL.
- **Ajouter continuellement à <a href="/sessions">sessions</a>**, c'est-à-dire insérer de manière incrémentielle les données nouvellement acquises par clé dans le champ. L'inconvénient est que si l'application commet une erreur, l'utilisateur est coincé avec les sessions et ne peut résoudre l'erreur d'aucune manière (sauf en supprimant les cookies, ce qui est extrêmement difficile pour la plupart des gens). De plus, avec un formulaire inachevé, il y a un risque que les données restent pré-remplies et puissent être vues par quelqu'un d'autre. Cependant, un cas bien pire se produit si la session a une validité très courte (disons 5 minutes) et que l'utilisateur perd les données de la première étape lorsqu'il remplit la dernière étape... cela peut alors devenir assez ennuyeux.

**Recommandé:**

- **Stockage de la base de données et passage des identifiants**. La première fois que le formulaire est soumis, nous stockons toutes les données collectées dans une table de base de données et générons un identifiant aléatoire (disons une chaîne de 10 caractères) à transmettre entre les pages comme paramètre. L'avantage de cette méthode est que, lors du traitement d'un formulaire, nous pouvons écrire les données nouvellement récupérées et validées directement dans la table et, en cas de défaillance, nous disposons de sauvegardes physiques des formulaires analysés et pouvons agir sur celles-ci. Par exemple, lorsqu'une commande est incomplète, nous pouvons envoyer un courriel à l'utilisateur pour lui signaler qu'il ne l'a pas terminée et augmenter les chances de vente.
- La fonction **Enregistrer dans le compte actuellement connecté** fonctionne exactement de la même manière que le transfert via l'ID, sauf qu'au lieu d'utiliser un identifiant aléatoire (jeton), nous utilisons une session vers l'ID de l'utilisateur actuellement connecté (s'il y en a un). L'avantage est que nous pouvons montrer les données pré-remplies à l'utilisateur de manière arbitraire dans le futur.

Conclusion
-----

Aucune de ces solutions n'est parfaite ou la seule correcte. Je combine moi-même plusieurs approches lorsque je travaille sur des solutions à plusieurs étapes. Typiquement, par exemple, je résous un panier d'achat comme une table de base de données, à laquelle j'affecte les données que j'ai déjà collectées et que je lie soit à un utilisateur (s'il est connecté) soit à une session (s'il n'est pas connecté et que nous ne nous connaissons pas encore).
