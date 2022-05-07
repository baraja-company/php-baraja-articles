Actions GitHub - le meilleur CI pour 2021
=========================================

> id: c1fe3b77-a506-4f20-bf1e-90ee82817c7d
> slug:
> 	cs: github-actions-nejlepsi-ci-pro-rok-2021
> 	fr: actions-github---le-meilleur-ci-pour-2021
> 
> perex: 'Protože už nějaký čas vyvíjíte webové aplikace, určitě jste si všimli, že se pro vás celá řada věcí rutinně opakuje, i když by nemusela. Velmi často jde o technickou správu projektů, verzování souborů, automatická kontrola kódu, zpracování různých dat, nebo třeba deploy na server a bezpečnostní věci kolem toho.'
> publicationDate: '2021-02-07 15:46:39'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: d3d151927a02dde744d1202449c904dc

Comme vous développez des applications web depuis un certain temps, vous avez probablement remarqué que de nombreuses choses sont systématiquement répétitives pour vous, même si elles ne devraient pas l'être. Très souvent, il s'agit de gestion de projet technique, de versionnement de fichiers, de révision de code automatisée, de traitement de données diverses, ou peut-être de déploiement sur le serveur et de sécurité.

En consultant dans les entreprises, je rencontre très souvent le problème où la prévention est très sous-estimée. La raison en est souvent que les développeurs perçoivent certaines choses comme très difficiles et que cela va leur ajouter du travail. Mais la vérité est que vous ne devez généralement mettre en place toute l'installation qu'une seule fois, puis récolter les avantages à long terme.

Qu'est-ce que CI (Continuous Integration)
----------

CI/CD est un outil qui permet d'automatiser les tâches de routine qui ont une base similaire et se répètent dans les projets. L'utilisation la plus courante de l'IC est l'exécution de tests automatisés, la révision du code, les déploiements automatisés (déploiement d'une application sur un serveur web), la gestion de projets ou encore les audits de sécurité.

Considérez l'IC comme un système d'exploitation virtuel qui exécute des commandes prédéfinies dans le terminal ou des programmes spécifiques. Le résultat de CI est soit un succès, soit un échec, qui est affiché directement dans le projet, mais aussi envoyé aux administrateurs qui peuvent y réagir. Une bonne pratique consiste à exécuter les tâches de CI à la suite d'un événement spécifique (par exemple, un commit dans le référentiel, une demande de fusion/demande d'extraction reçue, une étiquette/version/version, ou une exécution de timeloop cron).

Comment fonctionne spécifiquement l'IC
------------

La base de l'IC est un fichier de configuration dans lequel nous définissons son comportement et sa réponse aux événements. Dans le cas de GitHub, il suffit de créer un répertoire d'aide `.github/workflows` et de créer tout fichier avec l'extension `.yml` à l'intérieur. GitHub l'analysera à chaque livraison et l'exécutera si nécessaire.

Imaginons que nous voulions définir un nouveau fichier de configuration avec une tâche spécifique. Comme il s'agit d'une tâche qui sera exécutée directement par GitHub ou un autre environnement sur la machine virtuelle, nous devons définir des éléments de base concernant l'environnement, notamment :

- Le nom de la tâche (par exemple, le nom du test)
- Événements déclencheurs (l'événement qui doit déclencher la tâche, par exemple, chaque fois que vous poussez vers le référentiel ou que vous recevez une demande de pull).
- La définition des tâches elles-mêmes (il peut y avoir plusieurs tâches fonctionnant en parallèle).

Dans le cas des emplois, nous définissons plus précisément :

- Nom de l'emploi
- Environnement d'exécution (généralement le nom du système d'exploitation)
- Étapes de la construction du conteneur

Le conteneur ci-dessus est déjà la première préparation pour la **dockerisation complète du projet**. L'utilisation de base de CI est relativement facile et vous n'avez pas besoin de comprendre Docker du tout, il suffit de connaître les commandes dans Terminal.

Dans le cadre des étapes spécifiques de la tâche, nous devons exécuter les commandes individuelles qui vont construire l'installation du système d'exploitation que nous venons d'installer. Ainsi, dans le cas de PHP, il sera important d'installer uniquement le langage PHP (et de spécifier la version), de cloner le dépôt du projet sur le runner, d'installer les dépendances du projet (le plus souvent [Composer](/composer)), et d'exécuter le test lui-même.

Pourquoi utiliser GitHub Actions (CI) ?
--------

Une superstition répandue dans la communauté informatique veut que Microsoft soit une société maléfique qui fabrique des produits de mauvaise qualité. Dans le cas des actions GitHub, cependant, ce n'est pas vrai du tout, car ils ont, tout à fait objectivement, la meilleure plateforme CI du monde.

L'avantage fondamental de GitHub CI est la simplicité et l'élégance de la notation. En quelques lignes seulement, vous pouvez configurer votre propre environnement de test et le gérer automatiquement, même sans connaissances préalables. En même temps, je considère la vitesse de traitement de tâches spécifiques comme un avantage énorme. Par exemple, un test sur codestyle (formatage du code) et PhpStan (vérification des types de données, de la logique et de l'exactitude générale) peut être évalué par GitHub Actions sur un projet ordinaire en moins de 50 secondes, alors que GitLab, par exemple, évalue le même test en près de 8 minutes ! D'autres solutions comme Travis sont relativement coûteuses et la plupart des développeurs n'apprécient de toute façon pas les avantages de leurs fonctions spéciales.

Actions préparées
-----

GitHub dispose d'une vaste base de données d'actions et de paquets prêts à l'emploi que vous pouvez utiliser immédiatement. Il s'agit généralement de systèmes d'exploitation, de langages de programmation ou de bibliothèques de la communauté.

Vous pouvez trouver une base de données d'actions directement dans le [Marketplace] (https://github.com/marketplace?type=actions), par exemple mon action préférée est [Envoyer un SMS en cas de panne via Twillio] (https://github.com/marketplace/actions/twilio-sms).

Exemples de finitions
------

De très nombreux exemples [que je publie dans mes propres dépôts] (https://github.com/baraja-core) où vous pouvez voir de manière transparente quelle configuration spécifique j'utilise.

Par exemple, en février 2021, j'ai utilisé cette configuration pour vérifier le codestyle dans un projet :

```yaml
name: Coding Style

on:
  push:
    branches:
      - master
  pull_request:
    types: [ assigned, opened, synchronize, reopened ]
  schedule:
    - cron: '1 * * * *'

jobs:
  nette_cc:
    name: Nette Code Checker
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 7.4
          coverage: none

      - run: composer create-project nette/code-checker temp/code-checker ^3 --no-progress
      - run: php temp/code-checker/code-checker --strict-types --no-progress

  nette_cs:
    name: Nette Coding Standard
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 8.0
          coverage: none

      - run: composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
      - run: php temp/coding-standard/ecs check src
```

Ce fichier de configuration installe la dernière version d'Ubuntu (`runs-on : ubuntu-latest`), clone le dépôt du projet (`uses : actions/checkout@v2`), installe PHP (`uses : shivammathur/setup-php@v2`) version 7.4 (`php-version : 7.4`), installe le paquet code-checker, puis exécute le travail via PHP.

Quelques remarques supplémentaires pour les utilisateurs moins expérimentés :

- CI démarrera une machine virtuelle avec un système d'exploitation complet à chaque fois qu'il s'exécutera, qui pourra être utilisé pour appeler des commandes dans le Terminal tout comme, par exemple, votre serveur
- Pour vérifier les résultats des tests, on utilise des codes de retour tels que définis par Linux lui-même. Plus précisément, lorsqu'un programme renvoie `0` (zéro), cela signifie un succès et tout autre nombre signifie un échec. Par exemple, les scripts shell fonctionnent de la même manière
- Lorsque nous voulons écrire un test personnalisé ou une logique plus complexe, nous pouvons utiliser PHP, par exemple, et l'exécuter comme une tâche CLI (la commande `php file.php`), qui s'exécutera et pourra faire tout ce qu'elle a le droit de faire (travailler avec des fichiers, communiquer sur Internet, afficher à l'écran, ...).
- Pendant que CI fonctionne, toutes les sorties sont enregistrées à l'écran et peuvent être visualisées directement dans le navigateur web.
- CI n'est pas interactif, même si Terminal peut exécuter des opérations interactives avec l'utilisateur, CI ne le permet pas et doit être conçu comme une tâche qui démarre et se termine parfois.
- Un délai d'une heure est défini pour l'exécution du conteneur CI. Si tout n'est pas traité dans ce laps de temps (par exemple, le programme s'arrête), l'ensemble du système sera interrompu de force et une erreur sera signalée.
- GitHub peut exécuter des tâches de CI automatiquement par cron, mais très souvent, il ne les exécute pas au bon moment, mais avec un retard.
- Vous pouvez également envelopper toute la logique de CI dans un conteneur Docker et l'exécuter localement sur toutes les machines ou sur un serveur, par exemple
- Si vous devez accéder à des informations secrètes (par exemple, le mot de passe de la base de données), il existe une option permettant de définir des variables secrètes directement dans GitHub et CI les rend disponibles au moment de l'exécution.
- Le déploiement vers un serveur web est toujours mieux réalisé via SSH.
- La durée totale d'exécution des tâches de CI est limitée, généralement à 2 000 minutes par mois (très souvent, cela vous suffira, je ne l'ai jamais épuisé car une tâche ne dure qu'une minute au maximum), ou vous pouvez augmenter la durée moyennant des frais.
- Vous pouvez également héberger des exécutants CI sur votre serveur et contourner la limite de minutes, mais il est très probable que votre serveur ne sera pas beaucoup plus rapide, car Microsoft a beaucoup investi dans Azure, où GitHub Actions est hébergé et qui fonctionne sur des serveurs très puissants.
- Très souvent, il est pratique d'installer des paquets spécifiques juste pour CI (typiquement PhpStan et diverses fonctionnalités de sécurité), donc mettez-les dans la section `composer.json` du fichier `require-dev` afin qu'ils ne soient pas installés dans un projet spécifique comme une dépendance obligatoire.

Applications et bots GitHub
-----

Contrairement aux autres hébergeurs de dépôts, GitHub prend en charge les "GitHub Apps" et les robots d'automatisation. Il s'agit d'une grande base de données de robots prêts à l'emploi qui peuvent effectuer des opérations automatisées sur vos projets (même sans CI). Lorsqu'ils rencontrent quelque chose, ils déposent par exemple un problème ou envoient une demande de modification (pull request) avec une correction.

Je l'utilise personnellement et vous le recommande :

- [Dependabot](https://dependabot.com) - vérification automatique des dépendances dans Composer ; par exemple, lorsqu'une nouvelle version des paquets que vous utilisez sort et qu'elle est compatible, elle envoie automatiquement une demande de téléchargement.
- Codecov](https://github.com/marketplace/codecov) - vérifie la couverture du code à l'aide de tests automatiques et indique graphiquement les parties du code qui n'ont pas encore été couvertes.
- [Snyk](https://github.com/marketplace/snyk) - vérification automatique des vulnérabilités de sécurité
- [Imgbot](https://github.com/apps/imgbot) - optimisation automatique de la taille des données d'image

De nombreuses autres applications peuvent être trouvées sur le [Marché] (https://github.com/marketplace?type=apps).

Plus de conseils et d'astuces pratiques
-----

J'ai pris goût à l'utilisation des tâches d'automatisation dans GitHub.

J'ai mis en place des vérifications automatiques dans tous mes dépôts, de sorte que lorsque je soumets un commit (ou n'importe qui d'autre dans la communauté), j'obtiens un retour d'information en temps réel sur la qualité du code (codestyle et PhpStan), la sécurité, et plus encore, qui ont été violés.

J'ai des tests qui s'exécutent automatiquement toutes les heures. Par exemple, s'il y a une faille de sécurité ou un CVE, je le saurai dans une heure au plus tard, même au niveau des projets spécifiques et de ce qui s'est passé exactement.

Au fil du temps, après avoir testé différentes configurations, je suis arrivé à la conclusion que je considère les dépendances suivantes à Composer comme la configuration minimale idéale :

- `phpstan/phpstan` - vérification de l'exactitude et de la logique du code
- `tracy/tracy` - rapports d'erreurs et journalisation de haut niveau
- `phpstan/phpstan-nette` - Extensions et spécialités de Phpstan dans Nette
- `spaze/phpstan-disallowed-calls` - une bibliothèque brillante de Michal Spacek qui gère l'utilisation ou la non-utilisation de choses dangereuses dans le code (des vidages de variables oubliés à la possibilité d'exécuter une chaîne utilisateur comme commande shell).
- `roave/security-advisories` - vérifie l'installation de versions non sécurisées de paquets (si une vulnérabilité de sécurité est trouvée dans un paquet particulier ou si un CVE est publié, ce paquet empêchera l'installation de la version corrompue)

L'idéal est de configurer la sécurité de base pour qu'elle s'exécute automatiquement au moins toutes les huit heures et que les erreurs soient envoyées à votre courrier électronique. Car dès qu'une installation de projet échoue ou qu'une nouvelle vulnérabilité est découverte, je veux en être informé le plus rapidement possible et réagir immédiatement.

Rappelez-vous que tout fonctionne automatiquement. Vous pouvez donc être sûr que même si vous utilisez des processus compliqués pour vérifier la sécurité et d'autres choses, cela ne vous coûte aucun effort supplémentaire et vous avez juste besoin d'une configuration initiale bien exécutée.

Car le potentiel de la prévention et de l'automatisation est énorme !
