Réparation urgente d'un serveur surchargé
=========================================

> id: '07c5d1ed-f854-4797-8efe-350a24594762'
> slug:
> 	cs: senior-6-pretizeny-server
> 	fr: reparation-urgente-d-un-serveur-surcharge
> 
> publicationDate: '2023-02-11 14:25:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: f8239dd6b0eee234efc999c0dd3e0aeb

Un outil de surveillance externe vous indiquera que le temps de réponse moyen des 5 URL surveillées a doublé au cours des 30 dernières minutes. Le projet fonctionne sur un seul serveur physique qui n'est pas sous votre gestion et qui se trouve quelque part dans un centre de données. Vous vous connectez via SSH, lancez htop, et constatez que la charge du processeur est de 95% et que la mémoire déborde depuis longtemps.

Selon git, vous savez qu'il y a environ une semaine, ils ont effectué une migration de base de données vers une nouvelle structure de table, et un collègue écrit dans le chat qu'il a dû exécuter la migration pendant la nuit parce que le recalcul des colonnes et des index a pris environ 5 heures, pendant lesquelles presque toute la base de données était verrouillée, et ni INSERT ni SELECT ne fonctionnaient.

Les problèmes de performance sont donc probablement dus à des index mal conçus, à des requêtes SQL mal conçues ou à un pooling de connexions important. Il n'y a pas de temps pour un revert, il y a 7 mille utilisateurs sur le site selon Google Analytics, et une panne de 5 heures signifierait un risque de réputation pour le client, et une perte de dizaines à centaines de milliers de couronnes pendant ce temps (c'est difficile à estimer, les projectionnistes en font assez). Vous réalisez que tester uniquement les fonctionnalités sur un environnement de test n'est pas suffisant, et que vous devez également mettre en place un test de charge.

Comme il s'agit d'une importante boutique de commerce électronique de votre plus gros client et que vous vous attendez à ce que la situation s'aggrave, vous avez 30 secondes pour prendre une décision.

Comment procéder ?

1. vous ne faites rien. Le site sera plus lent, mais tant que le serveur peut le supporter, je suppose que c'est bon...
2. Vous allez essayer de préparer un plan pour rétablir la structure de la base de données, et l'exécuter dès que possible.
3. Vous faites migrer le site vers un serveur plus puissant (mais cela implique d'augmenter rapidement le prix pour le client, puis d'attendre peut-être 6 heures pour que la migration soit terminée, car vous avez des centaines de Go de tables de base de données).
4. Vous découvrez quelles requêtes SQL et quelles pages prennent le plus de temps, et vous les désactivez tout simplement.
5. Vous exécutez Cloudflare et le laissez vérifier statiquement ce qu'il peut.
6. Une autre magie (je ne pense pas qu'il y ait beaucoup à inventer)...
