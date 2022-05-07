Travailler avec des fichiers
============================

> id: '12330cfb-5729-419a-b2e4-cb3d1db998cc'
> slug:
> 	cs: prace-se-soubory
> 	fr: travailler-avec-des-fichiers
> 
> publicationDate: '2020-02-16 16:30:05'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '0f5b9e84a974285dcb63baafc912be5f'

Il existe un certain nombre de fonctions en PHP pour travailler avec les fichiers.

- <a href="/fopen">fopen()</a>, accès de bas niveau aux fichiers sur le disque
- <a href="/file-get-contents">file_get_contents()</a>, récupérer le contenu d'un fichier ou d'une URL.
- <a href="/file-put-contents">file_put_contents()</a>, sauvegarde d'une chaîne de caractères dans un fichier local.

Fonctions du disque
--------------

- `unlink($path)`, supprimer le fichier
- `copy($from, $to)`, copie un fichier d'un emplacement à un autre (peut être d'une URL au disque local)
- `rename()`, renomme ou déplace un fichier sur disque
- `chmod()`, changer les permissions de lecture, d'écriture ou d'exécution d'un fichier
