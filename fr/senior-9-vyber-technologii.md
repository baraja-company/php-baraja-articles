Comment sélectionner les technologies ? Quand passons-nous à JavaScript ?
=========================================================================

> id: d3ea7ea7-3e2f-455d-a424-cce374bcd1d2
> slug:
> 	cs: senior-9-vyber-technologii
> 	fr: comment-selectionner-les-technologies-quand-passons-nous-a-javascript
> 
> publicationDate: '2023-02-11 14:40:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '72807451867478e1a7b6d67f4e5c31b3'

Choisir les bonnes technologies est une condition préalable pour devenir un développeur confirmé. Ces décisions ne sont souvent pas faciles à prendre, car il faut tenir compte de l'état technique actuel de l'application, de l'orientation du développement, des connaissances de l'équipe actuelle, des connaissances courantes sur le marché du travail, des coûts de chaque technologie, des risques qu'elle présente pour votre activité, de la sécurité et de la stabilité de la technologie et, enfin et surtout, de l'intérêt des développeurs, disons dans cinq ans, lorsque 80 % de votre équipe actuelle aura été remplacée.

Je suis passé par 6 grandes entreprises qui développent en PHP. Seuls 2 d'entre eux essaient de passer à une autre technologie à long terme, les autres restent. Cela pose beaucoup de problèmes. Par exemple, j'essaie actuellement de trouver un développeur PHP senior pour un projet d'entreprise que je développe pour O2, avec l'obligation de faire la navette entre les bureaux de Prague, et je peux voir comment le marché des développeurs PHP s'est éclairci au cours des 5 dernières années. Le PHP n'est tout simplement plus cool et peu de gens veulent le faire. Il n'y a pas assez de juniors.

En interrogeant des personnes plus jeunes, je perçois que React et les technologies "légères" en général sont très populaires aujourd'hui. Du point de vue de l'architecture des applications, il est judicieux de découvrir cette orientation à un stade précoce et d'avoir le temps de s'adapter. Au lieu du martèlement complexe de mises en page et de formulaires web dans Latte, pour lequel vous avez besoin d'un développeur pratiquement médiocre pour une mission déjà légèrement plus complexe, dans React, vous avez juste besoin d'un junior qui a commencé fondamentalement il y a un mois, et qui ne fait toujours pas trop d'erreurs dans la solution future.

React vous permet de jeter une grande partie du backend qui a été écrit juste pour que le frontend puisse exister en premier lieu. En bref, il rend le développement moins coûteux et, en prime, vous obtenez une livraison plus rapide des nouvelles fonctionnalités, car les développeurs n'ont pas à traiter encore et encore les problèmes complexes qui découlent du langage de conception PHP.

La plupart des applications web n'ont même plus besoin d'un backend, ou seulement d'un backend minimal. Lorsque vous exposez des points de terminaison d'API dans Node.js (également une technologie basée sur javascript), un développeur qui ne faisait auparavant que du React peut soudainement écrire des éléments du backend, car il s'agit du même langage.

Dans une analyse plus approfondie des projets que j'ai développés au cours des 5 dernières années, il ne manque que quelques éléments dans Node.js qui me font encore utiliser PHP pour certaines opérations.

A savoir :

- Doctrine (et en général l'accès aux bases de données relationnelles basé sur des entités objets)
- Envoi et gestion des e-mails
- API SOAP (malheureusement, elle existe encore parfois)
- Sessions (vous devez le remplacer par un jeton JWT, par exemple)
- Une logique complexe écrite en PHP et que je ne peux pas facilement remanier.
- Traitement rapide de structures de données complexes où les données doivent être mutées.
- Les membres actuels de l'équipe que vous devez former à de nouvelles tâches.

Mais ensuite arrive Node.js, qui fait le reste des choses mieux. Par exemple :

- La possibilité de décharger l'application directement dans le nuage.
- Développement beaucoup (voire deux) moins cher de la même fonctionnalité
- Même logique sur BE et FE sans avoir à écrire le code deux fois
- Points de terminaison de l'API REST
- Appels parallèles à plusieurs codes à la fois
- Possibilité d'envoyer une réponse HTTP, mais le code continue à s'exécuter
- Crony
- Les bibliothèques doivent travailler avec les services en nuage
- Un temps de réponse nettement meilleur car vous n'avez pas à démarrer une énorme application.
- Paradigme entièrement fonctionnel (vous vous débarrassez des DIs dont vous n'avez pas besoin en JS, par exemple)
- Travailler avec des formulaires et des données
- Des mises à jour faciles et une communauté de développeurs active

**Commentaire sur Doctrine:** Je sais que JS apporte beaucoup de bibliothèques pour travailler avec des bases de données. Ou même de nouveaux paradigmes comme Mongo. J'aime la direction que prend le traitement des données. D'autre part, je pense que les bases de données relationnelles tabulaires ne disparaîtront jamais. Lorsque vous réalisez un projet de grande envergure et que vous gérez des dizaines de millions d'enregistrements, vous avez simplement besoin d'une technologie traditionnelle que vous connaissez bien et à laquelle vous savez à quoi vous attendre. Par exemple, l'idée que je veuille ajouter une colonne (propriété) et que cela implique de remapper toutes les entités avec un script de migration est assez effrayante pour moi.
