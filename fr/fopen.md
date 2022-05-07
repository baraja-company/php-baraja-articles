Fonction PHP fopen()
====================

> id: '733ba757-1d5f-4717-a088-5ddabe730838'
> slug:
> 	cs: fopen
> 	fr: fonction-php-fopen
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '6b791877e57d88840d603dea69967b69'

La fonction `fopen()` représente un accès de bas niveau aux fichiers sur le disque.

Le programmeur doit tout faire lui-même (ouvrir le fichier, lire les données, écrire de nouvelles données, fermer le fichier).

Si vous avez simplement besoin de lire et d'écrire des fichiers rapidement, il existe des options plus simples :

- <a href="/file-get-contents">file_get_contents()</a> - lecture d'un fichier
- <a href="/file-put-contents">file_put_contents()</a> - écriture dans un fichier

Utilisation de base
----------------

```php
$text = 'Tout texte qui est sauvegardé...';

$file = fopen('fichier.html', 'a+'); // Ouvre le fichier et le mode

fwrite($file, $text); // Sauvegarde dans un fichier
fclose($file);        // Ferme le fichier
```

> Si nous ouvrons un fichier en lecture et qu'il n'est pas fermé, aucun autre processus ne peut y accéder !

Types de modes de traitement des fichiers
----------------------------

Nous pouvons travailler avec des fichiers dans différents modes, qui donnent des informations sur les droits d'accès.

Par exemple, si nous voulons ouvrir un fichier en lecture seule, alors le mode `r` est suffisant.

Si nous ouvrons le fichier en écriture, il sera marqué comme `open` sur le disque et un autre processus (script) ne pourra pas y écrire jusqu'à ce que nous le fermions à nouveau. Cela garantit que le fichier ne sera pas corrompu pendant l'écriture.

| Mode | Signification |
|-------|--------|
| `et` | Ouvre le fichier, s'il n'existe pas il sera créé |
| `a+` | Ouvre un fichier pour ajouter des données et ou lire des données, s'il n'existe pas il sera créé |
| `r` | Ouvrir en lecture seule |
| | `r+` | Ouvrir pour la lecture et l'écriture |
| `w` | Ouvrir en écriture, les données originales seront supprimées et remplacées par de nouvelles données, si elles n'existent pas elles seront créées |
| `w+` | Ouvrir en écriture et en lecture, les données originales seront supprimées et remplacées par de nouvelles données, si elles n'existent pas elles seront créées |
