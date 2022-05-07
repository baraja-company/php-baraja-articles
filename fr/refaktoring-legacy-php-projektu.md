Refactoring d'un projet PHP hérité - comment rattraper la dette technologique
=============================================================================

> id: '8f37305a-e9dd-4b1e-9f53-04f0fca648f7'
> slug:
> 	cs: refaktoring-legacy-php-projektu
> 	fr: refactoring-d-un-projet-php-herite---comment-rattraper-la-dette-technologique
> 
> publicationDate: '2021-05-11 20:45:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: b13673b23a86bc82a88636e4a9464b4b

Lorsque je consulte des maîtres d'ouvrage avertis et expérimentés, je suis souvent confronté à la question de la viabilité à long terme d'un projet numérique. De nombreux grands projets qui dépassent les trois ans de développement commencent à devenir obsolètes en interne et ne sont plus viables. Imaginez maintenant une équipe de développeurs dont les niveaux de connaissances, d'expérience et, surtout, de diligence varient.

Afin de maintenir votre projet numérique au niveau TOP sur le plan technique, vous avez besoin :

- **Des développeurs compétents et un chef de projet** qui ont une excellente communication et où chacun sait ce qu'il fait et comment cela affecte les autres,
- **Outils fonctionnels et flux de travail** que toute l'équipe connaît et adapte son travail aux autres en conséquence,
- **Tâches automatisées** qui fournissent une routine quotidienne si facile à oublier qu'elle est extrêmement importante.

Si vous développez un projet de grande envergure, vous n'avez tout simplement pas la tâche facile. Il est important de faire les choses correctement, mais il est encore plus important de faire les bonnes choses.

Qu'est-ce que le remaniement et pourquoi en avez-vous besoin ?
---------------------------------------

Le concept de refactoring englobe un ensemble d'activités qui modifient l'apparence et la logique interne du code d'un programme sans affecter l'environnement. Vous pouvez considérer le processus de refactoring comme un nettoyage de Noël - tout reste pareil, mais c'est beaucoup plus organisé, et les choses inutiles sont jetées. En matière de refactoring, il est également important de garder à l'esprit qu'il ne s'agit pas d'un événement ponctuel, mais d'un processus à long terme que vous devez répéter à intervalles réguliers. Si vous ne le faites pas, vous vous exposez à toutes sortes d'erreurs étranges, de pertes financières, de problèmes d'intégration et de plaintes.

Si vous pouvez maintenir le projet stable et à jour, vous en tirerez de grands avantages :

- Le projet sera **plus sûr**. Des correctifs pour les bogues et les failles de sécurité sont publiés régulièrement dans les bibliothèques que vous utilisez. Vous devez aborder la sécurité, c'est l'une des fonctions vitales de base d'un projet sain.
- Le code et l'architecture interne deviendront plus simples et mieux pensés. Vous serez mieux à même de trouver les bogues, et de nombreux bogues peuvent même être évités.
- Avec un code simplifié, vous pouvez également faire appel à des développeurs juniors pour <a href="https://www.youtube.com/watch?v=1wZtdIb-Ppk">réduire considérablement</a> votre budget global.
- Vous constaterez peut-être que vous rendez certaines choses trop compliquées et trop compliquées - le projet sera simplifié dans son ensemble.

Pour rester sur la bonne voie dans un grand projet numérique, il faut courir. Mais tu veux grandir.

Comment refactorer en toute sécurité
-------------------------

Chaque refactoring est un gros pari qui peut ne pas être payant.

Je m'assure toujours d'un environnement stable bien avant de me lancer dans la refactorisation.

Pour la stabilité, vous avez surtout besoin d'une journalisation des erreurs **fonctionnelle**. Pour les projets simples, vous avez juste besoin de <a href="https://tracy.nette.org">Tracy</a>, qui consigne les erreurs dans un fichier HTML. Pour les projets plus avancés, des outils comme <a href="https://sentry.io/welcome/">Sentry</a> ou <a href="https://rollbar.com">Rollbar</a> entrent en jeu. J'utilise personnellement Tracy sur tous les projets, les autres outils dépendant du type de projet.

Environ un mois avant la refactorisation, je commence à suivre les bogues et à créer une feuille de calcul des bogues connus sur lesquels je me concentrerai plus tard. Il est toujours important de savoir quels bogues ont été introduits par le remaniement et quels bogues le projet contenait déjà dans le passé. Cela permettra ensuite de mieux défendre les résultats du travail auprès du client.

Une fois que je sais, grâce au journal, que je travaille sur un environnement relativement stable, nous pouvons passer aux choses sérieuses. D'autres rubriques décrivent les méthodes en fonction du risque qu'elles présentent.

Correction du style de codage et de la norme de codage
-----------------------------------

La plupart des projets n'ont pas un style cohérent de formatage des codes. C'est une grosse erreur. Un projet bien écrit ressemble au projet d'une personne, où tout est écrit de la même façon.

Les erreurs de formatage de base sont bien corrigées par l'outil <a href="https://doc.nette.org/en/3.1/code-checker">Nette Code Checker</a> que je passe régulièrement sur l'ensemble du projet.

Le style de codage, quant à lui, couvre la manière dont le code est formaté et l'indentation des différentes expressions du langage. La norme <a href="https://www.php-fig.org/psr/psr-12/">PSR-12</a> est très utilisée dans le monde du PHP, et de très nombreuses autres normes sont basées sur elle. Je recommande de l'utiliser.

Certains projets combinent maladroitement <a href="/spaces-and-tabs">tabs et espaces</a>. Pour éviter efficacement de briser la convention des espaces blancs (et d'autres erreurs), créez un fichier `.editorconfig` à la racine du projet qui indique à votre éditeur comment formater le code nouvellement écrit.

J'utilise personnellement cette configuration :

```txt
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.{php,phpt}]
indent_style = tab
indent_size = 4
```

Corrigez également toutes les <a href="/apostrophes-et-notes de mariage">notes de mariage en apostrophes</a>. Cela peut être fait automatiquement.

Vous pouvez également vérifier le formatage de votre code automatiquement, j'ai préparé une <a href="https://github.com/baraja-core/sandbox/blob/0db1f41068b6cedf79500c6a8b0ac26eae94a8eb/.github/workflows/coding-style.yml">démo entièrement fonctionnelle sur GitHub</a> pour cela. Si vous devez également corriger automatiquement le code, vous pouvez le faire avec le même outil. PhpStorm est également très bon pour corriger automatiquement le code directement, même en masse sur l'ensemble du projet.

Comment fixer le style de codage pour les grands projets
----------------------------------------------

Pour les grands projets, je vous recommande de procéder au formatage automatique du code un module à la fois, car cela peut créer un nombre énorme de conflits dans Git. Par conséquent, la meilleure stratégie est de corriger autant de lignes que possible avec un seul gros commit, de pousser ce commit directement sur le master, puis de distribuer le changement aux autres branches.

Si les conflits sont trop importants, il est logique de formater le code sur le master d'abord, puis à nouveau sur chaque grande branche où il y a beaucoup de changements. De très nombreuses lignes modifiées s'annuleront les unes les autres (faites une avance rapide), de sorte que vous résoudrez très peu de conflits, voire aucun.

Si vous ne faites rien, le code sera toujours mauvais. Les réécritures itératives sur plusieurs années ne fonctionnent absolument pas, car chaque fusion introduit la nécessité de corriger des dizaines de lignes où aucun changement n'a eu lieu, et cela complique inutilement la révision du code et les retours de changement potentiels.

PhpStan - analyse statique du code et correction des erreurs de type
------------------------------------------------------

Avant de se lancer dans une correction logique à grande échelle, il est très important d'examiner et de corriger les **caractéristiques de base du cycle de vie du projet**. Il s'agit notamment des éléments évidents que l'on attend de tout projet, mais qui ne sont pas toujours respectés.

Par exemple :

- Toutes les classes, méthodes et fonctions doivent exister
- L'héritage des classes ne doit pas être rompu
- Les classes doivent implémenter l'interface ou l'interface ancêtre utilisée. En même temps, vous ne devez pas hériter des classes `finales`.
- Vous ne devez pas appeler de fonctions non sécurisées telles que `eval()`, `shell_exec()`, `var_dump()` et ainsi de suite. Et si vous les appelez quand même, cela doit être explicitement indiqué dans le commentaire
- Vous devez toujours attraper les exceptions et ne pas laisser l'application entière tomber en panne.

La solution à ce problème est d'installer <a href="https://phpstan.org">PhpStan</a> dans le projet et de le corriger **au moins au niveau 1**. Oui, c'est difficile, et oui, c'est beaucoup de travail. Mais si vous ne le faites pas, chaque remaniement devient une roulette russe et le développeur espère simplement que les dégâts seront aussi minimes que possible.

Le niveau de base de PhpStan n'exige pas, par exemple, que les types de données et autres choses formelles existent partout, ce que nous aborderons dans le futur. D'autre part, il se peut qu'une fonction ou une méthode ne renvoie pas un certain type de données mais affirme autre chose dans le typehint.

Rector - réparation itérative sûre
-----------------------------------

Un dicton de programmation bien connu dit qu'avant d'ajouter une nouvelle erreur au système (comme le lancement d'une exception), nous devons d'abord ajouter cette erreur en tant qu'avertissement, et seulement ensuite la transformer en erreur fatale si elle ne se manifeste pas pendant un long moment.

Il en va de même pour les types de données. Lorsque je refactore du code inconnu, je laisse des outils automatiques comme <a href="https://getrector.org">Rector</a> ajouter d'abord des annotations de commentaires au code, ce qui ne casse rien, mais aide à clarifier ce qui sera où plus tard. Ces commentaires sont ensuite écoutés par PhpStan, qui peut être utilisé pour vérifier que nous n'avons rien cassé et qu'il s'agit d'une modification sûre.

En général, j'ajoute des commentaires aux propriétés et aux arguments en un seul commit, puis j'attends peut-être un mois, et lorsque tout fonctionne à long terme, j'ai recours à la réécriture vers des types de données fixes aux endroits où il n'y a pas eu de problème à long terme.

Voir l'article <a href="https://tomasvotruba.com/blog/2019/07/29/how-we-completed-thousands-of-missing-var-annotations-in-a-day/">Comment nous avons complété des milliers d'annotations @var manquantes en un jour</a>.

Pour en savoir plus sur <a href="https://github.com/rectorphp/rector">Rector, voir GitHub</a>.

Mise à jour des dépendances
----------------------

Avant de se lancer dans la mise à jour des dépendances des paquets ou même des versions de PHP, il est important de rechercher et d'apprendre à utiliser <a href="/github-actions-best-ci-for-2021">les tests automatisés</a>.

Si les tests fonctionnent, nous pouvons nous rendre dans l'outil <a href="/composer">Composer</a> pour effectuer la mise à jour. Si vous le pouvez, demandez l'aide d'un robot <a href="https://dependabot.com">Dependabot</a> qui met à jour automatiquement le `composer.json` de votre projet et peut vérifier les changements de compatibilité.

Si vous avez un grand nombre de modifications à apporter, faites-les toujours lentement et incrémentez-les version par version. Ne mettez jamais à jour plusieurs paquets à la fois. Après chaque mise à jour, analysez l'ensemble du projet avec PhpStan et corrigez les bogues. C'est un processus long qui prend plusieurs heures, mais les enjeux sont importants.

Où aller ensuite
-------

Les prochaines étapes sont individuelles en fonction du type et du statut du projet. En général, un code bien conçu et écrit par des développeurs chevronnés est mieux entretenu que celui acheté à bas prix par un junior qui a travaillé dans une agence de médias où le développement web n'est qu'une activité secondaire, pour ainsi dire.

Croisez les doigts ! Ça va être dur, mais tu vas t'en sortir.
