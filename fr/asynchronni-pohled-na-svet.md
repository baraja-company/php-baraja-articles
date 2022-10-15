Vision asynchrone du monde
==========================

> id: '7ed25269-659f-4d17-a642-d07480cbfafa'
> slug:
> 	- null
> 	fr: vision-asynchrone-du-monde
> 
> cs: asynchronni-pohled-na-svet
> publicationDate: '2022-10-15 11:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '24d4ec106e7febec5ee84cdf86bd8f28'

J'ai remarqué depuis longtemps que notre monde a une nature asynchrone et décentralisée. Lorsque vous vous en rendez compte et que vous commencez à réfléchir à la manière de l'utiliser à votre avantage, un grand concept de résolution de problèmes complexes peut facilement émerger. Dans ce billet, j'aimerais expliquer certaines des idées que j'utilise déjà. Je ne les tire pas d'une source particulière, c'est plutôt une combinaison de multiples expériences et de mes propres idées. Ces principes ne fonctionnent pas pour tous les cas.

Définir l'environnement et les objectifs
-------------------------

Presque tous les projets auxquels je me suis attaqué (seul ou en équipe) avaient un objectif assez précisément défini. Cette approche est très logique car il est important de savoir ce que nous voulons. D'un autre côté, je pense que définir un objectif spécifique est impossible dans un environnement complexe, et souvent, au cours de la mise en œuvre, nous nous apercevons que nous voulons en fait arriver ailleurs.

Ainsi, au lieu d'un objectif spécifique, je construis une zone d'objectifs différents où il existe des alternatives, ou même un objectif indépendant isolé complètement nouveau peut être le bon objectif s'il a du sens.

Exemples tirés de la pratique :

- Je dois m'étendre progressivement à d'autres marchés. L'un des sous-objectifs que j'ai découvert au cours de la mise en œuvre pourrait être la traduction automatique du web.
- Je dois récupérer 6 envois à différents endroits de Prague et je ne connais pas le chemin le plus court. Je ne veux pas le découvrir de manière compliquée, alors je me dirige simplement vers le point le plus éloigné et je change mon plan vers d'autres routes en cours de route. Par exemple, lorsque je change à Anděl, je décide de prendre le tramway qui arrive en premier, ce qui affecte dynamiquement le plan de route suivant.
- Je dois écrire ce post, mais je n'ai pas une heure de temps d'affilée. Par conséquent, je peux l'écrire en plusieurs parties tout en prenant le métro, comme des chapitres individuels. Je ferai ensuite la fusion finale dans ce formulaire à l'avenir.
- Je ne sais pas comment fonctionne l'algorithme de calcul du remboursement de la marchandise pour mon client, que je dois reprogrammer. Je poserai donc la question à plusieurs personnes en même temps et je résoudrai autre chose pendant que je le ferai. De manière asynchrone sur 2 jours, de multiples réponses différentes me parviendront, sur lesquelles je ne prendrai une décision que plus tard.
- Je dois me débarrasser de PHP sur le serveur pour une longue période, donc je réécris progressivement une page à la fois en React. Je déplace progressivement les sites vers le cloud et je les construis sur des Nect générés statiquement.

Aucune de ces destinations n'est une destination finale. Si vous travaillez avec une surface et non une pointe, vous avez beaucoup plus de chances de la toucher. En même temps, la motivation fonctionne bien ici, car la pire chose qui puisse arriver est d'atteindre votre objectif et de ne pas avoir quelque chose à attendre avec impatience à l'avenir.

Il est également important de dire que pour avoir un état d'esprit similaire, vous devez disposer d'un environnement assez stable où vous pouvez faire des erreurs. Cette approche ne peut pas garantir un délai spécifique pour résoudre une tâche unique spécifique. D'autre part, il peut optimiser la résolution d'un maximum de tâches parallèles en un minimum de temps, mais pas nécessairement dans l'ordre où elles sont arrivées dans la file d'attente.

Le principe pourrait être comparé à la façon dont les calculs fonctionnent dans la programmation parallèle, par exemple. En bref, vous décomposez les tâches en fils individuels qui résolvent différentes sous-tâches à la fois, et une fois que vous avez obtenu toutes les réponses, vous pouvez composer la solution.

Déploiement de nouvelles versions de logiciels
--------------------------------

J'ai beaucoup de mal avec ça partout.

La façon dont le développement de logiciels est généralement géré est la suivante : vous avez une version stable qui fonctionne en production pour les utilisateurs, puis vous avez un environnement de test où le nouveau développement est effectué, et de temps en temps vous faites passer les nouvelles de l'environnement de test à la production, ce qui s'appelle une version. Ce processus est relativement sûr et lent.

Comme je suis progressivement passé à une architecture micro-frontale, où le front-end de l'application (ce que voit l'utilisateur) est complètement séparé du back-end (où se déroulent les calculs et le traitement des données), le processus de publication peut fonctionner différemment.

J'ai encore un environnement de production qui fonctionne toujours. À partir de là, une nouvelle branche est créée pour chaque tâche visant à traiter une fonctionnalité spécifique. Comme j'utilise Vercel (un très bon fournisseur de services en nuage) pour héberger le front-end, je peux avoir une URL personnalisée pour chaque branche de développement.

Dès qu'une nouvelle fonctionnalité est testée, elle est immédiatement fusionnée et déployée en production dans un processus asynchrone. Par conséquent, il arrive fréquemment qu'il y ait 15 déploiements de nouvelles versions par jour.

Tout ceci est en grande partie lié à une idée qui m'a été transmise au LMC : " Une fonctionnalité que vous fournissez à un client demain, même si elle est imparfaite, a beaucoup plus de valeur pour lui qu'une fonctionnalité parfaitement déboguée mais qui ne sera disponible que dans un an ". Mais cela ne peut fonctionner que si vous disposez d'un moyen de diffuser rapidement les nouvelles - typiquement le développement web.

Ce que j'aime beaucoup dans le framework Next.js (construit sur Recto) et Vercel, c'est le principe que vous connectez votre dépôt GitHub directement à Vercel, et à chaque commit il y a un build automatique, des tests, puis un déploiement en production quand tout est ok. Le développeur n'a donc à se soucier de rien, et l'application est déployée toutes les heures sans aucun effort. Il est très important de formaliser les choses courantes, puis de les automatiser.

Publication de contenu
----------------

J'ai décomposé des dizaines de sujets et de postes supérieurs avec moi dans Apple Notes. Pour chaque sujet, je trouve continuellement de nouvelles idées à noter et à classer par tags. Lorsque plusieurs notes sont générées pour un sujet, je les convertis en chapitres, puis je convertis le groupe de chapitres en articles et en messages FB.

Caractéristiques de ce principe :

- Je ne sais pas à l'avance combien d'articles je vais publier,
- Mais là encore, je sais qu'il peut y en avoir beaucoup,
- Je suis en mesure de m'assurer que chaque billet est rempli d'informations, de points de vue et d'idées qui ont mûri au fil du temps (ce billet a pris environ 3 mois à mûrir),
- C'est durable et je m'en réjouis car je n'ai pas à écrire une tonne de texte en une seule fois et à risquer d'oublier quelque chose.

Et puis le processus de publication.

Je publie d'abord beaucoup de contenu discrètement sur le web, afin que Google le remarque en premier et que la page soit indexée. Lorsqu'il a une bonne couverture par les mots-clés et que je suis satisfait de l'ensemble, le sujet est diffusé sur les médias sociaux, par exemple.

Pour beaucoup de sujets, je sais qu'ils ne seront pas intéressants et qu'ils risquent plutôt de vous ennuyer. Mais en même temps, je veux les avoir en ligne parce que quelqu'un pourrait les rechercher à l'avenir. Dans ce cas, l'article reste uniquement sur le web. Un exemple de ces articles est constitué par les différents articles superposés, qui relient l'ensemble du domaine thématique afin de couvrir le plus grand nombre de sujets possible sur le site. Les articles de couverture sont souvent plus techniques et ennuyeux. Ou bien il s'agit de catégories générées par une machine, dans lesquelles je me contente d'organiser plusieurs articles en thèmes, couvrant ainsi le plus grand nombre possible de mots-clés, que quelqu'un pourrait ensuite vouloir rechercher.

Connaissance, éducation, tests
------------------------------

J'aime jouer avec des technologies dont on ne sait pas dès le départ à quoi elles serviront un jour.

Comme la traduction automatique. À première vue, cela n'a pas de sens de traduire des dizaines d'articles en langues étrangères pour des lecteurs avec lesquels je ne suis pas en contact. En revanche, il peut s'agir de l'une des étapes d'une décision future pour laquelle les conditions préalables doivent être remplies.

En général, personne ne sait à quoi ressemblera l'avenir. Il me semble donc tout à fait logique de couvrir le plus grand nombre possible de possibilités que vous souhaitez comprendre, au moins superficiellement, et peut-être aborder à l'avenir. Il est bon de considérer les étapes non pas seulement comme une tâche résolue, mais comme un processus évolutif sans fin qui n'a pas de destination finale.

Cette approche fonctionne-t-elle pour la réalisation de projets personnalisés ?
--------------------------------------------------------

La plupart du temps, non.

Vous devez avoir un client qui est votre partenaire et vous voulez tous deux fournir la meilleure solution possible, même si vous savez dès le départ que vous ne la découvrirez pas avant la fin.

Dans notre secteur, cela s'appelle le développement agile, et ces principes s'en inspirent. D'autre part, j'ai observé que le développement agile tel que je le connais ne me convient pas directement, et j'aime proposer des solutions de rechange courbes.

Avec l'agilité, je vois beaucoup le principe de l'engagement en termes de qui résout quoi dans un sprint particulier. Il me semble plus logique de construire l'environnement de telle sorte que vous ayez un énorme arriéré de tâches qui sont résolues en fonction de ce qui est le plus nécessaire sur le moment, ou de ce que j'ai la capacité mentale de faire. Lorsque je suis sur la route, par exemple, j'ai tendance à m'attaquer à des tâches de réflexion plus développées que je peux faire à l'extérieur sans ordinateur. Lorsque je suis de nouveau au bureau, je m'attaque à des tâches plus intensives en algorithmes et en mathématiques, car je peux me concentrer pleinement et investir beaucoup d'énergie mentale.

Je ne recommande pas ce principe si vous créez une toute nouvelle boutique en ligne ou si votre entreprise est en train de faire faillite. Vous avez besoin d'un environnement stable qui fonctionne tout seul, et vous ne ferez que des bijoux. Mais en même temps, vous savez que si vous ne faites rien, votre environnement stable prendra fin un jour.
