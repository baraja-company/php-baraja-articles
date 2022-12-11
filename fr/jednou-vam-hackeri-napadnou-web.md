Un jour, des pirates informatiques attaqueront votre site web
=============================================================

> id: a11ca0a6-77e1-4ba4-8eca-83449c9cbf48
> slug:
> 	cs: jednou-vam-hackeri-napadnou-web
> 	fr: un-jour-des-pirates-informatiques-attaqueront-votre-site-web
> 
> publicationDate: '2020-08-04 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '0f0ac4c091115e9c1f0833d318f34f5d'

Ça ne va pas t'arriver ? :DDDDDDD

En gérant plus de 300 sites web, je suis confronté de temps en temps à diverses urgences. Parfois, elles sont très animées, mais souvent, elles sont d'une banalité totale. Si, comme moi, vous avez été tenté par la programmation dans le passé, et que vous savez aussi que [la programmation est une souffrance] (http://borisovo.cz/programming-sucks-cz.html), vous serez certainement d'accord avec moi.

Sous l'emprise des hackers maléfiques
----------------------

Lorsqu'une application web devient populaire, elle devient une cible tentante pour les pirates. Leur motivation n'est généralement pas de faire tomber l'ensemble du site web, bien au contraire. En fait, **les pirates veulent que vous, en tant qu'administrateur du serveur, ne soyez pas du tout au courant de leur existence**. Un bon pirate attend pendant des mois, surveillant le serveur web et obtenant la chose la plus précieuse - vos données.

Pour protéger vos utilisateurs, il est impératif de crypter toutes les données et de disposer de plusieurs couches de protection. Pour les mots de passe, utilisez l'une des fonctions lentes telles que [Bcrypt + sel](https://php.baraja.cz/hashovani), Argon2, PBKDF2, ou Scrypt. Michal Spacek a déjà écrit sur [security rating] (https://pulse.michalspacek.cz/passwords/storages/rating#slow-hashes), et je le remercie de compléter cet article. En tant que propriétaire de site, vous devez insister sur le hachage correct du mot de passe lorsque vous discutez avec un programmeur. Sinon, vous allez pleurer, comme beaucoup de gens avant vous qui se sont aussi fait l'illusion que cela ne les concernait pas.

Pouvez-vous dire quand vous êtes attaqué ?
---------------------------------

J'ai remarqué que lorsqu'un serveur web atteint un certain seuil de trafic (en République tchèque, c'est environ 50+ visiteurs par jour), il commence à recevoir une série d'attaques qui sortent de nulle part. Pour compliquer les choses, l'attaquant a toujours un avantage, car il peut choisir la partie de l'infrastructure qu'il va commencer à attaquer et vous - en tant que propriétaire du serveur web - devez le reconnaître à temps, vous défendre et être mieux préparé pour la prochaine fois.

Combien d'attaques ont été dirigées contre vous au cours du dernier mois, par exemple ? Vous le savez exactement ? Combien d'entre eux avez-vous défendu ? Est-ce que quelqu'un d'autre a accès à votre serveur ?

Et si quelqu'un vous attaquait en ce moment même ? Et maintenant... et maintenant aussi... ?

Malheureusement, il n'existe pas de méthode unique pour reconnaître une attaque. Mais il existe des outils qui peuvent vous donner un avantage significatif.

Ce qui a le mieux marché pour moi dernièrement :

- **Lorsqu'un attaquant ne sait pas où se trouvent physiquement vos serveurs**, sa position est beaucoup plus difficile. Les informations relatives à l'architecture physique peuvent être très bien masquées grâce à [Cloudflare] (https://www.cloudflare.com/), qui protège (et crypte de manière bidirectionnelle) toutes les communications réseau. Il présente également un certain nombre d'autres avantages, tels qu'une récupération plus rapide depuis l'étranger, une protection automatique contre les attaques DDOS, la compression des actifs et, enfin et surtout, le blocage automatique des visiteurs malveillants. J'utilise Cloudflare pour tous mes sites web (et presque chaque projet utilise des technologies différentes).
- **Error logging**. Toujours et tout. Il est fort probable que votre application contienne de nombreux bogues commis par votre programmeur. Qu'il s'agisse de [uncaught exceptions] (https://php.baraja.cz/vyjimky), d'erreurs d'application, d'injection SQL, etc. Si vous programmez en PHP et que vous ne comprenez pas la journalisation, il y a suffisamment de problèmes que [Tracy](https://tracy.nette.org/) (et éventuellement d'autres outils comme [Sentry](https://sentry.io/)) et les journaux d'accès du serveur lui-même peuvent détecter. L'enregistrement des erreurs n'est pas une option intéressante, c'est une nécessité. Je consigne les bogues sur tous les sites dont je m'occupe activement et je reçois les informations sur les nouveaux bogues par courrier électronique afin d'en être informé immédiatement.
- **Plate-forme sécurisée et mise à jour**. Votre site est-il équipé de WordPress ? Savez-vous comment le mettre à jour correctement ? Connaissez-vous tous les risques auxquels vous êtes confrontés ? Quand quelque chose est gratuit, c'est presque toujours au détriment d'autre chose. Personnellement, j'utilise le [Nette framework] (https://nette.org/cs/) version 3 pour le développement d'applications, que je mets à jour chaque semaine et que je recherche activement les failles de sécurité à corriger. Tout comme vous faites réviser votre voiture régulièrement, vous devez faire réviser votre site web régulièrement et au moins une fois par mois. La maintenance du site comprend la mise à jour de toutes les bibliothèques externes et la surveillance constante des journaux. Si vous utilisez une ancienne version d'une bibliothèque, il est très probable qu'un attaquant connaisse la vulnérabilité et tente de l'exploiter.
La question est de savoir quand
Un grand nombre de personnes dans ma région sous-estiment la sécurité de manière incroyable et exposent sur Internet des contenus très faciles à casser et à exploiter.

En fait, même les grandes entreprises font des erreurs, même sur des choses banales comme l'attaque [XSS](https://www.zive.cz/clanky/vyvojar-objevil-zranitelnost-v-seznamu-dokazal-mezi-vysledky-propasovat-zakazany-kod/sc-3-a-200023/default.aspx) (cross-site scripting).

La question n'est pas de savoir si quelqu'un va un jour casser votre site web. La question est de savoir quand cela se produira et si vous serez suffisamment préparé pour y faire face. Peut-être qu'un pirate a eu accès à votre serveur depuis des mois et que vous ne le savez pas (encore).

Que faire en cas de problème
-------------------------------

Lorsque vous découvrez le travail du hacker, il est généralement trop tard et le hacker a fait tout ce qu'il devait faire. Il a très probablement placé des logiciels malveillants partout sur le serveur et a au moins un contrôle partiel de la machine.

Il s'agit d'un problème de type **chaotique et vous devez agir rapidement**.

Comme il s'agit de la dernière étape de l'attaque, je recommande personnellement de déconnecter complètement le serveur web d'Internet et de commencer à comprendre progressivement ce qui s'est passé. De plus, nous ne saurons jamais exactement si le serveur cache quelque chose d'autre. L'attaquant a toujours une bonne longueur d'avance.

Si nous décidons de remettre la dernière sauvegarde fonctionnelle sur le serveur, nous aiderons beaucoup l'attaquant. Il sait déjà comment s'introduire dans votre serveur et rien ne l'empêche de le faire à nouveau dans quelques heures. En outre, la sauvegarde contient probablement un logiciel malveillant ou un autre type de virus.

Bien qu'il s'agisse d'une option très impopulaire parmi mes clients, je recommande toujours **la suppression complète des sites WordPress** en premier lieu, ou de tout système que le client ne met pas à jour et qui présente de nombreuses menaces de sécurité. Par exemple, si vous hébergez sur un même serveur 30 sites datant de plus de deux ans, vous êtes pratiquement assuré qu'au moins l'un d'entre eux présente une certaine forme de vulnérabilité.

Si vous ne comprenez pas le problème, il vaut mieux contacter très tôt un spécialiste approprié qui a une connaissance approfondie de votre problème et qui comprend les choses dans un large contexte.

**Note éthique:**

Si vous gérez un site web pour un client qui a connu un incident de sécurité, il est de votre responsabilité de l'en informer. En cas de fuite de données (comme les mots de passe, mais souvent bien plus), vous devez également informer les utilisateurs finaux que leur mot de passe est de notoriété publique et qu'ils doivent le changer partout. Si vous utilisez une fonction de hachage lente (par exemple, **Bcrypt + sel**), il sera beaucoup plus difficile pour un attaquant de craquer les mots de passe (malheureusement, [les mots de passe Bcrypt peuvent être craqués](https://arstechnica.com/information-technology/2015/08/cracking-all-hacked-ashley-madison-passwords-could-take-a-lifetime/)). Si vous souhaitez recevoir des informations régulières sur les fuites de votre compte, je vous recommande de vous inscrire sur [Have i been pwned ?](https://haveibeenpwned.com/). Pour plus d'informations sur [la faille de sécurité] (https://m.uoou.cz/vismo/zobraz_dok.asp?id_ktg=5020&n=poruseni-zabezpeceni), veuillez consulter le site Web de OOOO.

Les habitudes atomiques
--------------

Au cours des six derniers mois, j'ai terminé la lecture du livre [Atomic Habits] (https://www.melvil.cz/kniha-atomove-navyky/), qui m'a ouvert les yeux. Il décrit un processus d'amélioration progressive de 1 % chaque jour qui fera beaucoup à long terme.

Comme je suis approché par des entreprises et des particuliers à différents stades de la dette technologique, j'aimerais résumer les pires choses :

- **FTP pour la communication avec le serveur n'a pas de sens**. Il s'agit d'un protocole non sécurisé qui est contrôlé par un mot de passe. Si vous voulez donner accès à quelqu'un, il doit connaître le mot de passe et peut faire la même chose que vous. Si vous voulez lui retirer l'accès, vous ne pouvez pas, le seul moyen est de changer le mot de passe. Il est préférable de passer complètement à SSH et d'utiliser des clés RSA. Je suis souvent surpris que même les entreprises résolvent ce problème de nos jours. Ne faites jamais un tableau de tous les mots de passe. Et certainement pas une partagée par l'ensemble de l'équipe !
- Le **Code source se trouve dans Git**, les données dans la base de données et le contenu statique (images, PDFs, ...) dans des fichiers sur le disque. Le choix d'un mauvais stockage de données aggrave la conception et la sécurité de l'application. L'URL d'un PDF (par exemple d'une facture) doit contenir un contenu généré de manière aléatoire que seul le destinataire connaît. Pour les contenus extrêmement sensibles (comme un relevé bancaire), il est important de chiffrer le contenu sur le disque et de ne le fournir qu'après la connexion.
- Les parties non publiques du site doivent contenir l'en-tête META `noindex` et `nofollow` pour éviter d'être indexées par Google. Le contenu sensible que vos concurrents ne doivent pas connaître doit être protégé par un mot de passe.
- Désactivez [directory content listing] (https://www.simplified.guide/apache/disable-directory-listing) sur votre serveur. S'il est configuré de manière inappropriée, l'ensemble du site peut être exploré par une machine pour trouver des fichiers sensibles.
- Racine du projet ! Il ne doit jamais y avoir d'URL directe vers les fichiers de configuration. Par exemple, [Nette does it right](https://doc.nette.org/cs/3.0/quickstart/getting-started#toc-obsah-web-projectu) dès le premier tutoriel. Il en va de même pour les [dépôts git publiquement disponibles] (https://smitka.me/open-git/). Ce type de vulnérabilité est extrêmement facile à sous-estimer et les conséquences sont souvent tragiques.
- Le bogue peut également provenir d'une erreur de développement involontaire. Il est très important de faire une revue de code par un humain en chair et en os pour chaque changement. De nombreux bogues sont découverts par [PHPStan](https://github.com/phpstan/phpstan) ou des outils similaires avant qu'un humain ne voie le code.
- Ne jamais [envoyer des mots de passe sous une forme lisible par courrier électronique] (https://www.lupa.cz/clanky/reset-a-poslani-hesla-v-citelne-podobe-e-mailem-nebezpecna-praktika/), il y a toujours un autre moyen de résoudre le problème.
- Problèmes de sécurité dans les anciennes versions des bibliothèques et des logiciels. Les robots parcourent l'internet chaque seconde et attaquent les failles de sécurité connues (injection SQL, XSS, CSRF, ...). Beaucoup de failles connues ont des versions plus anciennes de WordPress.

Les vulnérabilités sont bien plus nombreuses et les problèmes varient d'un projet à l'autre. Si vous avez besoin de faire un audit rapide de votre serveur, je vous recommande d'utiliser [Maldet] (https://www.rfxn.com/projects/linux-malware-detect/) et de faire appel à un spécialiste pour vous aider à faire un [audit complet du site] (https://baraja.cz/audit-webu), et pas seulement pour des raisons de sécurité.

Les ingénieurs en sécurité sont comme le plastique dans l'océan - juste pour toujours. Il faut s'y habituer.

Le principe d'action et de réaction de la nature
-------------------------------

Vous aurez également remarqué que la nature utilise presque toujours le principe de réaction. Cela signifie que quelque chose se produit dans un environnement particulier et que les organismes environnants y réagissent de différentes manières. Cette approche présente de nombreux avantages, mais un énorme inconvénient : vous serez toujours à la traîne.

En tant que personne réfléchie (concepteur, développeur, consultant), vous disposez d'un avantage considérable, à savoir la possibilité d'agir, c'est-à-dire de connaître à l'avance une certaine partie des grands risques et de les empêcher activement de se produire. Vous n'éviterez peut-être jamais toutes les erreurs, mais vous pouvez au moins en atténuer les conséquences, ou mettre en place des mécanismes de contrôle qui détectent les problèmes à un stade précoce afin de pouvoir réagir.

La plupart des attaques se déroulent sur une longue période, et si vous enregistrez les données, vous aurez généralement suffisamment de temps pour détecter le problème.
