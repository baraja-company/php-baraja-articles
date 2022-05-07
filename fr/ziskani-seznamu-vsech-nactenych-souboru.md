Obtenir une liste de tous les fichiers chargés
==============================================

> id: '72716cbc-90a6-4870-848b-125e8430707f'
> slug:
> 	cs: ziskani-seznamu-vsech-nactenych-souboru
> 	fr: obtenir-une-liste-de-tous-les-fichiers-charges
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '7e896aa0e501d0fe33dc6595c1bfb43e'

Lors du débogage d'applications plus complexes, il arrive parfois que je ne sache pas quels fichiers ont été chargés et si quelque chose manque.

Si vous utilisez Composer ou tout autre type de <a href="/autoloading-trid">class autoloading</a>, vous ne connaissez probablement pas ce problème. Cependant, il peut s'agir d'une situation relativement courante lors du débogage d'anciennes applications d'autres développeurs.

Obtenir tous les fichiers chargés peut être fait avec la fonction `get_included_files()`, qui les retourne comme un tableau de chaînes de chemin absolu.

Il est courant, dans le cadre du développement, d'avoir un nombre considérable de fichiers chargés (par exemple, même ce blog relativement simple utilise près de 160 fichiers). La plupart du temps, cependant, le volume important n'a pas d'importance car le contenu du fichier est récupéré à partir d'OPCache, qui est optimisé pour les lectures en masse.
