Application sûre
================

> id: cc4667a3-aea3-4e0a-b323-da31ab78e019
> slug:
> 	cs: bezpecna-aplikace
> 	fr: application-sure
> 
> publicationDate: '2019-10-01 14:19:04'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '74e52382b995b303550d8e4b783bbb84'

Si vous voulez sérieusement développer des applications web et que le site sera plus tard disponible sur l'internet, il est très important d'aborder la question de la sécurité.

De façon réaliste, les menaces suivantes guettent les développeurs :

- **L'application présente une erreur interne**, par exemple parce que le programmeur a commis une erreur qu'il n'a pas remarquée en écrivant le code, ou qu'elle ne se manifeste que "parfois".
- **Le serveur a une mauvaise configuration** ou l'environnement **a changé** parce que l'administrateur du serveur a modifié le comportement du serveur et les sites n'étaient pas adaptés pour cela. Ou bien, nous déployons sur une nouvelle machine et ne connaissons pas la configuration exacte.
- Quelqu'un essaie d'attaquer**, qu'il s'agisse d'un attaquant externe ou d'un ancien employé du système.

Toutes les situations sont extrêmement désagréables et nécessitent une action immédiate. Concevoir une application pour qu'elle ne tombe jamais en panne n'est pas techniquement possible.

Pendant le développement, il est très important de tester le code une fois qu'il a été écrit, et si un bug survient sur le serveur, il est important d'en informer immédiatement le développeur avec une description précise du problème.

Pour détecter les bogues et surveiller la santé du serveur, je recommande l'outil <a href="https://tracy.nette.org/">Tracy</a>, qui consigne toutes les erreurs fatales, les exceptions non gérées et autres dans des fichiers directement sur le disque du serveur. En outre, vous pouvez configurer une adresse électronique à laquelle les notifications seront envoyées.

Vous souhaitez obtenir une consultation sur la sécurité de votre application ?

Contactez-moi.
