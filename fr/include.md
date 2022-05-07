PHP Include - insertion d'un fichier dans une page
==================================================

> id: '7a53145c-8552-425d-b864-283f73a7a7de'
> slug:
> 	cs: include
> 	fr: php-include---insertion-d-un-fichier-dans-une-page
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '6e18e67ec03b3a7592473230559fc364'

La construction `include` insère automatiquement des fichiers/scripts supplémentaires dans la page courante.

En ce qui concerne PHP, il sera automatiquement exécuté et compris comme s'il avait toujours été à cet endroit.

Exemple :

```php
include 'news.html';
```

Utilisation pratique
-----------------

- Menus communs et menus,
- Des nouvelles, des mises à jour, des nouvelles et le même contenu,
- En-tête ou pied de page, ...

Chargement dynamique de fichiers
--------------------------

Nous avons souvent besoin de charger des fichiers de manière dynamique, par exemple en fonction d'une variable.

Par exemple :

```php
$clanek = 'neco';

include $clanek . '.html';
```

Ou nous pouvons charger dynamiquement des articles dans la page :

```php
include 'articles/' . $_GET['page'] . '.html';
```

Lorsqu'une URL avec le paramètre `page` est appelée, l'article est automatiquement inséré, par exemple : `https://example.com/?page=novinky`.
