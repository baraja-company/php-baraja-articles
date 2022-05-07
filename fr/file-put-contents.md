Fichier_sortie_contenu
======================

> id: '7ec781c6-e3db-484d-8a49-0b93ab8252b1'
> slug:
> 	cs: file-put-contents
> 	fr: fichier-sortie-contenu
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '78211a3696ebba56d559045f8a0ab9e4'

La fonction **file_put_contents** est adaptée à l'écriture automatique dans un fichier. Sinon, vous pouvez aussi utiliser <a href="/fopen">fopen()</a>, ce que je ne recommande pas aux débutants.

Echantillon
--------------------------

```php
$file = 'file.txt';
$content = 'Contenu à sauvegarder dans un fichier.';

file_put_contents($file, $content);
```

file_put_contents a 2 paramètres :

- `filename` où écrire,
- Le `contenu du fichier` que nous allons écrire.

> Note : `file_put_contents()` écrase le fichier avec le dernier contenu.

Attention à l'écrasement
--------------------------

Si vous enregistrez via file_put_contents, attention à l'écrasement des données. La fonction supprimera tout le contenu actuel et le remplacera par le nouveau contenu. Ainsi, si vous souhaitez simplement ajouter le texte, vous pouvez l'ajouter au début ou à la fin en utilisant votre propre script :

```php
$file = 'file.txt';
$content = 'Nouveau contenu.';

$oldContent = file_get_contents($file);
file_put_contents($file, $content . $oldContent);
```

Donc, d'abord le fichier est ouvert, puis le nouveau contenu est écrit, et le contenu original est écrit après...

Si nous voulons ajouter l'ancien contenu avant le nouveau, il nous suffit de modifier légèrement le script :

```php
$file = 'file.txt';
$content = Nový obsah.';

$oldContent = file_get_contents($soubor);
file_put_contents($file, $oldContent . $content);
```
