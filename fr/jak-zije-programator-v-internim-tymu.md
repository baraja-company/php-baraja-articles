Comment vit un programmeur dans une équipe de développement interne
===================================================================

> id: '9309b0f2-0265-43aa-a9c3-10b2d486cdfc'
> slug:
> 	cs: jak-zije-programator-v-internim-tymu
> 	fr: comment-vit-un-programmeur-dans-une-equipe-de-developpement-interne
> 
> publicationDate: '2022-01-10 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: e65dd78fa9813b552f30b98f04aacb30

Il y a un lourd prix à payer pour l'honnêteté.

Ce site a toujours été une description de la réalité vécue par les personnes travaillant dans le secteur des TI. J'aimerais donc parler de mon expérience au sein d'équipes de développement. Voici les expériences générales que j'ai vécues dans différentes entreprises. Aucune expérience n'est associée à une entreprise particulière et ne constitue nécessairement une critique.

Les entreprises ont tendance à ne pas vouloir de personnes travailleuses et proactives.
----------------------------------------------

Vous avez beaucoup d'idées ? Voulez-vous innover ? Vous aimez trouver des solutions élégantes aux problèmes complexes que votre équipe résout et qui accablent la moitié de l'entreprise ? Vous êtes conscient de l'importance de la sécurité, de la conception des logiciels et de la recherche des goulots d'étranglement des projets ?

Vous serez probablement malheureux dans l'équipe de développement pendant un long moment.

Le travail d'équipe est une chose avec laquelle j'ai beaucoup de mal ces derniers temps - je veux dire que je nage précisément dedans et que j'essaie de comprendre les principes totalement inintuitifs auxquels je dois adhérer.

- Le travail d'équipe consiste à élaborer une solution que l'ensemble de l'équipe comprend. Ce n'est souvent pas le meilleur. Ce n'est presque jamais élégant. En fait, c'est toujours inefficace. Mais elle présente l'avantage cool que toute l'équipe la comprend et qu'elle peut être gérée sur le long terme. C'est une idée extrêmement importante. Dans le cas des grands projets, il n'y a pas de pression pour être efficace, car l'entreprise peut s'appuyer sur le personnel et dispose de suffisamment d'argent. Il est beaucoup plus important de livrer les choses à temps, même si elles sont partiellement cassées et avec des contraintes inconnues à l'avance, mais à temps. Il s'agit de l'idée que la solution que vous livrerez demain (même si elle est imparfaite) a beaucoup plus de valeur pour le client qu'une solution déboguée à 100 % qui ne sera pas disponible avant un an.
- Innover et repousser les choses, c'est plutôt mal. C'est lié au fait qu'il y a de vraies personnes qui travaillent dans des entreprises, qui ont des familles et qui ne veulent travailler que pendant les heures de bureau. Il s'ensuit nécessairement que les gens ont un temps limité pour apprendre et essayer de nouvelles choses. En fait, les entreprises doivent faire en sorte que les heures de travail des employés soient consacrées à l'éducation et au développement qui peuvent être utilisés pour un travail futur. Une conséquence intéressante est alors le report des décisions et de l'apprentissage de nouvelles choses au moment nécessaire. Pour ma part, je comprends cette approche et elle me paraît très logique. Si j'avais des employés, je préférerais aussi des personnes calmes qui resteront dans l'entreprise pendant, disons, 15 ans, plutôt que des personnes qui ont une bonne vue d'ensemble et veulent passer à autre chose, mais ce sera au détriment de l'équipe. Il est vrai que la solution collective ne sera jamais la même que celle de l'individu, mais cela ne veut pas dire qu'elle sera pire.

Les personnes comme moi sont une source potentielle de problèmes et de conflits. C'est cool d'y penser de cette façon.

Quand vous faites une revue de code, cherchez juste les erreurs objectives.
----------------------------------------

Chaque entreprise a ses règles de codéveloppement. Malheureusement. Ça m'a tellement ennuyé au début.

Lorsque vous utilisez l'un des langages les plus matures, comme .NET, les règles de codage ont tendance à être plus similaires. Ce n'est qu'alors que vous découvrez qu'il existe encore du PHP et du JavaScript dans le monde. L'aspect PHP, en particulier, est parfois un peu frustrant. PHP est un langage très mature, doté de nombreuses fonctionnalités intéressantes, mais d'après mon expérience, il est utilisé dans les entreprises à environ un tiers de son potentiel total. Les raisons sont diverses, le plus souvent l'inertie.

À cet égard, je cherche depuis longtemps une solution procédurale à la révision du code. Je joue avec PhpStan depuis longtemps et je suis les idées qui l'entourent. L'idée de base de PhpStan est qu'il ne met en évidence que les erreurs objectives qui sont certaines de se produire à un moment donné. Plus j'explore PhpStan et l'amène au niveau supérieur, plus je valide intérieurement la véracité de cette affirmation.

Ce serait bien si PhpStan était implémenté dans au moins la moitié des entreprises tchèques. Il serait encore mieux d'atteindre au moins le niveau 6. Dans la pratique, j'ai vu les meilleures entreprises avoir le niveau 4 ou 5.

L'autre jour, je me demandais s'il était possible d'avoir une véritable application de production en cours d'exécution qui dispose de sa propre équipe de développement, et qui ne passe même pas le niveau 0 - c'est-à-dire le niveau qui contrôle les choses que l'on attend de toute application. Il s'agit de s'assurer que toutes les classes et fonctions existent, que les fichiers n'ont pas d'erreurs d'analyse, etc. Et oui, il existe de telles applications. Mais la situation s'améliore à long terme. Je l'espère.

Je suis donc une approche similaire avec codereview. Je ne signale que les choses qui sont objectivement toujours fausses. Je rencontre souvent des erreurs de jugement de degré - quelque chose ne va pas, mais encore une fois, ce n'est pas un problème si important qu'il faille le traiter. De nombreux problèmes sont résolus par convention, ou en déclarant que le risque est faible et qu'il est donc inutile de le traiter.

J'ai toujours considéré les types de données manquants (même composés) comme un bogue critique. Puis j'ai compris qu'il y avait un test et une décharge.

Je ne pense pas avoir une conclusion concrète à cette partie. J'ai juste beaucoup de commentaires subjectifs à ce sujet, qui sont plus susceptibles d'être une source de malentendus mutuels, donc je ne les posterai pas. À long terme, je suis enclin à conclure que je ne peux pas faire de revue de code - je suis soit trop strict, soit trop bienveillant. Je ne peux pas dire à un niveau général ce qui est vraiment important et ce qui ne l'est pas. Je serais très intéressé de savoir comment les autres personnes s'y prennent. Personne n'a encore été capable de me donner une réponse que je puisse utiliser moi aussi.

Il n'y a pas d'argent dans le développement personnalisé.
---------------------------------

Avez-vous une agence et faites-vous du développement web personnalisé ? Si vous n'êtes pas assez important et ne réalisez pas de grands projets pour d'autres sociétés, vous n'existerez probablement pas un jour.

Cela s'explique par le faible financement des projets personnalisés. Cela a probablement à voir avec la nature des contrats, qui sont plus rentables pour les acheteurs. En fait, la plupart des agences sont brutalement sous-financées et ne font que des bénéfices réels pour les propriétaires.

Lorsque vous développez un projet en interne en tant qu'entreprise pour vous-même, un retard de livraison d'un mois n'a probablement pas beaucoup d'importance. En tant qu'agence, si vous ne livrez pas un travail dans un délai d'un mois, vous êtes assuré de vous endormir lentement au travail ce mois-là, de ruiner constamment votre santé, de voir le client jurer dans le téléphone et les billets, de toujours demander si cela pourrait être plus rapide, et en guise de récompense, vous paierez toujours une pénalité contractuelle. Et si vous ne le faites pas, le client vous considérera comme un entrepreneur peu fiable et pourra envisager d'aller ailleurs, où il ne sera de toute façon pas mieux.

Mais ce sont les règles du jeu, que j'ai appris à connaître dans la pratique, et qui ont fait un trou dans mon budget.

La sous-traitance est probablement la forme d'activité la plus complexe dans le domaine de l'informatique. Vous ne voulez pas faire de contrats. Le contrat vous détruira. Ils vous appelleront le week-end, la nuit, à Noël. La moitié du travail que tu fais, tu n'es pas payé. Tu vas t'excuser pour des erreurs que tu ne peux pas faire. Vous regarderez sur Instagram le storytelling d'amis qui sont partis pour leurs troisièmes vacances cette année, tandis que vous serez assis dans votre bureau à 3 heures du matin un samedi, en train de terminer un projet client pour lequel vous vous ferez de toute façon engueuler le lundi parce que le contrat contenait plus de choses que vous ne pouviez en faire. Les gens prendront des vacances et des congés maladie et vous travaillerez à leur place. Ou vous êtes licencié. Et ensuite vous serez heureux de payer le loyer.

Mais là encore, on apprend beaucoup. Parce que sous la pression, vous avez beaucoup de pression pour apprendre et être efficace afin que la prochaine fois vous puissiez en faire un peu plus dans le même laps de temps. Le problème est que vous ne pouvez pas vraiment appliquer ces connaissances et cette expérience ailleurs, car les entreprises ne sont tout simplement pas intéressées (comme je l'ai expliqué ci-dessus).

Heureusement, il s'agit d'une histoire de 2019 et c'est loin d'être le cas.

Il n'y aura jamais de monde idéal
-------------------------

Si je fais ce que j'aime ? Et vous ? :D

Je suppose que c'est une question de proportion. Vous ne ferez probablement jamais un travail qui vous satisfasse à 100%. Vous aurez probablement toujours quelqu'un qui vous expliquera que vous ne pouvez pas faire la chose que vous avez apprise avec force pendant 10 ans et que vous savez intérieurement que vous pouvez faire.

D'un autre côté, en renonçant à votre propre personnalité, vous gagnez l'espace nécessaire pour avoir quelque chose de plus. Comme le temps de week-end, une meilleure santé, plus de temps pour soi, un environnement stable, etc. Le fait qu'il ne puisse pas être soutenu à long terme est également évident.

J'aime pouvoir essayer différentes choses pour voir comment les autres vivent. Travailler dans une équipe de développement est très ennuyeux par rapport à un travail sur mesure.

Il ne s'agit plus de trouver la solution la meilleure et la plus rapide, mais de collaborer. Il s'agit d'améliorer la communication, les émotions et, surtout, d'apprendre à être humain. Et c'est une grande valeur, aussi.
