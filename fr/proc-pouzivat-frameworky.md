Pourquoi et comment utiliser les frameworks et les bibliothèques
================================================================

> id: '5ea6cdde-f67c-4f3f-8871-37b27ee8e23a'
> slug:
> 	cs: proc-pouzivat-frameworky
> 	fr: pourquoi-et-comment-utiliser-les-frameworks-et-les-bibliotheques
> 
> publicationDate: '2020-02-16 22:13:46'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '0926b7ec4f59904c008388431ff22eb5'

Une blague célèbre dit que les programmeurs ne commencent à utiliser des frameworks que lorsqu'ils écrivent le leur et constatent qu'il ne mène nulle part. Le plus drôle dans tout ça, c'est que c'est vrai. J'en ai fait l'expérience moi-même. Deux fois, même.

Cependant, la page principale de <a href="https://nette.org">Nette</a> dit :

> Les vrais programmeurs **n'utilisent pas de frameworks**. Ils écrivent des applications web via la ligne de commande directement sur le serveur. C'est un hommage qui leur est rendu. Pour le reste d'entre nous, Nette rendra notre travail beaucoup plus facile et plus agréable.

Le rôle des frameworks dans le développement d'applications
-----------------------------------

Je suis sûr que vous le savez vous-même :

Vous avez l'idée d'un grand projet et vous commencez à écrire du code directement en PHP pur... au bout de quelques heures, vous vous rendez compte qu'une grande partie du travail est encore répétitive et que vous résolvez des problèmes de base du système. Par exemple, comment se connecter à une base de données, comment rendre un formulaire, comment valider des données, comment envoyer un courriel, et bien plus encore.

Vous écrirez probablement vos propres fonctions à appeler pour ces tâches. Si vous écrivez plusieurs projets à la fois, vous commencerez à partager ces fonctions entre les projets dans un grand fichier désordonné, et vous les modifierez continuellement selon vos besoins.

Et lorsque les fonctions sont à un certain stade de développement et que vous commencez à construire un nouveau projet par dessus à chaque fois et à développer directement, alors vous pouvez parler d'avoir écrit votre propre framework, ou au moins une bibliothèque.

Qu'est-ce qu'un cadre et que fait-il ?
-------------------------

Comme nous l'avons déjà montré avec un exemple concret tiré de la vie, un framework est une collection de nombreuses fonctions (mais idéalement de classes et d'objets) qui épargnent le travail du programmeur. Lorsqu'il développe, il ne doit pas réfléchir à la manière de se connecter à une base de données, mais il sait que c'est déjà programmé quelque part et que "ça marche tout simplement".

Les cadres finis sont donc le fruit de l'intelligence de centaines de personnes qui ont développé des milliers d'applications sur une longue période afin de déboguer une solution et un écosystème pour s'attaquer à de nouveaux projets.

Avec les frameworks, vous obtenez également un ensemble de **meilleures pratiques**, qui sont des moyens de résoudre les problèmes sans avoir à y réfléchir. Si vous rencontrez un problème, quelqu'un l'a probablement résolu avant vous, et il est toujours préférable de trouver une solution dans la documentation que de devoir la résoudre soi-même de manière compliquée.

La programmation abstraite et le principe d'encapsulation
---------------------------------------------

Les frameworks bien conçus comme Nette vous permettent de programmer à un **très haut niveau d'abstraction**.

De cette façon, le programmeur (l'utilisateur du framework) n'a pas à comprendre ce qui se passe en interne et comment les composants fonctionnent exactement en interne, ce qui lui fait gagner beaucoup de temps et d'efforts. Il peut se concentrer sur la résolution des problèmes réels de son application et obtenir des résultats très rapidement.

David Grudl lui-même (l'auteur du framework Nette) a dit un jour qu'il avait conçu Nette pour pouvoir programmer un site web pour un client en une nuit, lorsqu'il rentre tard du pub et doit lancer le site web le matin. Cela s'explique principalement par le fait que le développeur n'est pas du tout concerné par les aspects techniques.

Pour que cela fonctionne et que le développeur puisse simplement utiliser le framework fini dans son ensemble, il est très important d'utiliser correctement le <a href="/encapsulation">principe d'encapsulation</a>.
