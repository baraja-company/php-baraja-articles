Comment faire face aux plantages soudains des scripts PHP
=========================================================

> id: '41edbf1b-3ca5-487f-a54c-fac9926bc3d2'
> slug:
> 	cs: senior-7-sessions
> 	fr: comment-faire-face-aux-plantages-soudains-des-scripts-php
> 
> publicationDate: '2023-02-11 14:30:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: bdd1e207ee9c0ba19c39f0b5c7d83c43

Une histoire de fin 2016, où j'ai été littéralement sauvé par un collègue : dans une application PHP, vous décidez d'enregistrer des images via un script proxy, qui, entre autres, peut ajuster leurs dimensions et d'autres paramètres en fonction de la requête entrante. Dans le cadre de l'optimisation, vous sauvegardez également les variantes générées physiquement sur le disque.

Cependant, dans le cadre d'une opération de production, vous commencez soudainement à voir une charge énorme et des milliers de demandes en attente. Les images sont chargées séquentiellement, une par une, pour chaque utilisateur. Les rafraîchissements de la page et les clics sur les liens ne fonctionnent pas. L'application semble complètement figée. Il suffit d'attendre que tout soit traité.

Quel pourrait être le problème ? J'ai listé 3 indices majeurs dans le texte pour permettre une recherche rapide du problème. Hotfix a une solution triviale.
