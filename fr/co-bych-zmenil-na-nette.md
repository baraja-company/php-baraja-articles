Ce que je changerais à propos de Nette si j'étais David Grudl
=============================================================

> id: bad8fe4f-92a4-4396-8698-7ec42bb2aef8
> slug:
> 	- null
> 	fr: ce-que-je-changerais-a-propos-de-nette-si-j-etais-david-grudl
> 
> cs: co-bych-zmenil-na-nette
> publicationDate: '2022-11-18 17:45:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '953e6c78b9dff185bbf8e3e9582d24c3'

J'utilise activement Nette Framework depuis presque 6 ans pour le développement de logiciels commerciaux. Au cours des premières années, j'étais très enthousiaste et le cadre répondait parfaitement à tous les besoins de l'équipe, et il n'y avait pratiquement aucune raison de chercher un autre outil.

Depuis environ 2019, je commence à constater une certaine baisse de l'enthousiasme initial au sein de Nette, où certaines des fonctionnalités avancées commencent à me manquer. Il s'agit souvent de choses qui dépassent le cadre lui-même, et je ne m'attends donc pas à ce qu'elles soient un jour mises en œuvre, mais d'un autre côté, un développeur doit prendre une décision sur la façon de poursuivre le développement à l'avenir, et peut-être opter pour une technologie différente.

Couche de base de données
-----------------

Je considère que la base de données Nette est l'une des plus grandes erreurs du framework, malheureusement.

Chaque semaine, nous organisons des entretiens au sein de l'entreprise, au cours desquels des juniors sortant tout juste de l'école, ou même des personnes intermédiaires en transition depuis un autre cadre, postulent et découvrent Nette Database dans le cadre de leur exploration de Nette. En tant que couche de base de données, ce n'est pas trop mal, mais le problème est alors l'utilisation pratique dans des projets réels.

C'est parce que Nette Database ne fait aucun effort pour pousser les développeurs à utiliser des types de données stricts dès le départ, à nommer les colonnes et les relations, à séparer les méthodes de requête (référentiel) des modèles, et d'autres petites nuances qui font beaucoup dans l'ensemble.

Pendant de nombreuses années, j'ai utilisé la bibliothèque Dibi, qui a joué un rôle énorme à son époque. Il vous permet d'interroger la base de données de façon très agréable et unifie l'interface. D'un autre côté, les temps ont changé, et lorsque les bases de données renvoient des objets non typés, PHPStan ne sera pas heureux par principe.

Personnellement, je voudrais soit incorporer un support natif pour les entités et les dépôts dans la base de données Nette, soit fournir un support officiel pour Doctrine dans Nette. Si vous programmez des applications au niveau d'une grande entreprise, vous devez être sérieux en matière de développement, et Doctrine en particulier a beaucoup de sens. Dans le même temps, vous souhaitez former immédiatement les nouveaux arrivants à une meilleure technologie et vous ne voulez pas leur inculquer de vieilles habitudes.

Tracy et enregistrement des erreurs
---------------------

Je considère toujours que Tracy est le meilleur débogueur d'exécution pour PHP.

Lorsque je suis arrivé dans une agence numérique sans nom en 2017, et que j'ai vu l'état technique de leurs projets Nette, j'ai été horrifié. Combien de fois ont-ils eu des sites écrits en pur PHP sans aucune tentative d'enregistrer les erreurs. Une solution assez facile était d'installer Tracy sur le projet, d'appeler `Debugger::enable()` et de ne rien faire de plus. Les bogues étaient automatiquement enregistrés dans un répertoire qu'il suffisait de ramasser manuellement tous les quelques jours.

Quant à Tracy, il me semble qu'il s'agit d'un produit fini que David a peu de raisons d'améliorer. En revanche, je vois encore des possibilités d'amélioration dans une nouvelle direction.

Par exemple, pourquoi Tracy n'inclut-il pas des fonctionnalités payantes pour les entreprises ou les particuliers qui prennent la sécurité au sérieux ?

Tracy pourrait mettre en place une interface web personnalisée de type Sentry, où vous créez un compte et où les bogues de production sont automatiquement envoyés via une API où vous pouvez les suivre sur l'ensemble des projets. L'application pourrait également interroger des sites individuels, détecter automatiquement leur indisponibilité et en informer le propriétaire par d'autres moyens (puisque Tracy ne peut pas logiquement détecter une panne de serveur). Il m'arrive aussi de rencontrer le problème suivant : Tracy est accidentellement activé sur un site de production, et vous pouvez découvrir via le DIC comment accéder à la base de données ou ailleurs. Tracy devrait également vous alerter à ce sujet.

Tracy pourrait également mettre en œuvre d'autres petites choses que je connais de Node.js. Par exemple, pour l'affichage du code source, il pourrait être possible de choisir l'intervalle de caractères auquel la ligne est mise en évidence d'une manière particulière, d'où la possibilité de mettre en évidence un token spécifique dans une ligne. En même temps, il serait bien d'ajouter la prise en charge d'une deuxième ligne de surlignage (peut-être bleue) pour indiquer le contexte de la partie suivante de l'application.

Lorsque j'affiche un extrait de code source, je n'hésiterais pas à ajouter un bouton capable de rendre le reste du code en ajax afin que je puisse l'explorer directement dans le navigateur à partir de la pile d'appels. C'est ce que fait Symfony, et c'est une excellente fonctionnalité. Si cela était complété par une analyse statique de base avec la possibilité d'explorer les méthodes, les classes et l'héritage, cela permettrait d'économiser beaucoup de travail de débogage pour toute l'équipe.

Si la pile d'appels traverse un paquet, il serait agréable d'afficher la version du paquet avec lequel je travaille pour un fichier particulier. Il arrive souvent qu'une erreur survienne dans un paquet que je suis en train de développer, et je pourrais tout aussi bien savoir visuellement qu'il s'agit d'une ancienne version.

Routage statique
----------------

Dans Nette Router, je permettrais de définir un nouveau type appelé routeur statique. Il s'agirait d'un descendant du routeur de base, mais il ne pourrait accepter qu'une chaîne littérale, qui ne serait pas une expression régulière, mais un chemin direct. De nombreuses pages fonctionnent de manière statique (contacts, divers articles, et même, étonnamment, la page d'accueil), ce qui oblige le routeur à évaluer à chaque fois les règles regex pour la correspondance et la composition des URL.

Si une route statique était utilisée dans un modèle Latte, par exemple, elle pourrait être traduite en toute sécurité en une chaîne statique `{$basePath}/path` au moment de la compilation du modèle, de sorte qu'il ne serait pas nécessaire de compiler les 100 URL qui se trouvent dans la mise en page, par exemple, à chaque requête.

Je comprends que je peux obtenir la même fonctionnalité en mettant en cache le modèle, mais j'apprécierais beaucoup plus une solution système.

Dans le cadre de Next.js, j'ai complètement craqué pour le concept de routage statique. Il n'existe pas de `n:href`, en bref, il faut connaître l'URL à l'avance, ce qui vous oblige à ne pas faire autant de redirections, à penser aux formats d'URL à l'avance, et à les garder persistants.

Interface PSR
------------

Je trouve dommage que Nette n'implémente pas nativement l'interface PSR. En tant que développeur de paquet, cela vous égare si vous voulez définir une dépendance de Composer sur Nette. D'un côté, Nette résout de nombreux problèmes pour vous, de l'autre, vous savez que vous ne pourrez plus la remplacer par la suite. Plus d'une fois, je me suis retrouvé à préférer utiliser Symfony, Laravel ou une interface générique complètement différente en raison de l'absence d'une interface générique.

L'absence de prise en charge des normes génériques pour la portabilité future est la principale raison pour laquelle j'ai complètement abandonné Nette en 2022 et que nous développons de nouvelles applications en équipe exclusivement en générique.

Couche API
----------

Que faire si vous voulez utiliser Nette comme backend pour traiter les demandes d'API REST ? Est-ce que vous construisez un présentateur et envoyez la réponse comme `$this->sendJson()` ? Allez-vous installer une bibliothèque tierce ? Ou êtes-vous David Grudl qui résout le besoin d'une bibliothèque manquante en en écrivant une nouvelle immédiatement ?

Pourquoi Nette ne peut-elle pas implémenter quelque chose d'aussi courant que l'API REST dès le départ ? Aujourd'hui, presque toutes les applications ont besoin de communiquer via une API, par exemple pour fournir des données au front-end.

Pour les nouveaux arrivants, cela les incite à utiliser le Latte et les snippets intégrés plutôt que de découvrir qu'il existe une meilleure option, à savoir construire l'intégralité du frontend séparément et se contenter de lui fournir des données.

Faire confiance à ce qui se passera dans 10 ans
-------------------------

Le dernier point est très pragmatique et provient d'un débat que nous entendons de temps en temps de la part des départements commerciaux de l'équipe.

Que se passerait-il si David Grudl arrêtait complètement de développer Nette ? Et si quelqu'un arrêtait de le développer ? Que ferions-nous ? Et quel serait le défi à relever ?

De nombreux développeurs (qui n'ont souvent aucune expérience en entreprise) aiment se moquer de cette préoccupation, en disant que ce n'est pas grave. Comme ouais, mais ça ne l'est pas.

Lorsque vous êtes confronté à la question de savoir s'il faut investir du temps dans Nette ou dans React et Node.js pour former les jeunes développeurs de votre entreprise, c'est malheureusement la dernière option qui l'emporte.

Nette n'a pas encore raté le train, mais dans les dizaines d'entretiens que j'ai eus au cours des six derniers mois, je le ressens assez fortement.
