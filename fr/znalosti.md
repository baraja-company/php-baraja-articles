Aperçu des connaissances en matière de développement web
========================================================

> id: e5e9613c-66fe-4d5e-a686-a182aab8061a
> slug:
> 	cs: znalosti
> 	fr: apercu-des-connaissances-en-matiere-de-developpement-web
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7284b1fc11b40ddb7628995070198a1

Il ne suffit pas de savoir comment créer un site web et en prendre soin de manière exhaustive. Le chemin est semé d'embûches et il est bon d'avoir au moins une idée de base de chaque chose. Quand j'ai commencé, je ne savais pas vraiment ce qu'il fallait apprendre. Cette page sert de repère à travers les sujets que j'ai dû étudier progressivement pour être en mesure de comprendre le développement web et de faire face aux situations les plus courantes.

Administration du serveur
--------------

> Un serveur web est un ordinateur qui fait fonctionner le web. Lorsqu'un utilisateur consulte une page, le serveur web lui envoie la page.
>
> À l'heure actuelle (2021), il n'est plus judicieux d'opter pour un hébergement gratuit si vous vous intéressez sérieusement au Web. Un serveur peut être **loué à partir de 50 couronnes par mois**.

- Installation du serveur (diffère sous Windows et Linux)
- Configuration du serveur
	- Paramètres d'Apache
	- Configuration PHP
	- <a href="/info">récupération de la configuration PHP actuelle</a> (fonction `phpinfo()`)
	- Routage Ngnix (améliore les performances du serveur)

Internet et navigateur web
--------------------------------

- Navigateurs web
- Principe de demande et de réponse
	- Adresse URL
	- Ajax et Ajaj
	- Génération de code HTML (systèmes de templating)

Cordes
-----------------

- Lecture, écriture et concaténation de chaînes de caractères, notamment le travail de base sur les chaînes de caractères.
- Traitement des chaînes de caractères
	- Naviguer personnage par personnage
	- <a href="/if">comparaison de chaînes de caractères</a>
	- Similitude des chaînes de caractères (fonctions `levenshtein()`, `similar_text()` et `soundex()`)
	- <a href="/explode">Explode</a>, séparation par un séparateur
	- Les expressions régulières** divisent les chaînes de caractères en fonction d'un masque universellement configurable.
	- **Tokenizer** décompose les chaînes de caractères en petites parties (tokens).
- Sérialisation des données dans une chaîne de caractères
	- <a href="/json">Json</a>, un objet javascript stocké dans une chaîne de caractères
