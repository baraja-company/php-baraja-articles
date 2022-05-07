Inclure (plier les pages des pièces)
====================================

> id: '4984832e-c11f-4e9e-8d3b-60561685389d'
> slug:
> 	cs: include-soubor
> 	fr: inclure-plier-les-pages-des-pieces
> 
> publicationDate: '2019-08-23 15:06:33'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: aea5867cf788e623dddd3ced4ea24a7a

PHP est à l'origine un langage de modélisation, créé pour faciliter l'assemblage de morceaux de pages.

Formats pris en charge
-------------------

Le pliage fonctionne sous forme de texte, il est donc conseillé d'utiliser des formats pertinents tels que `.html` ou `.md`.

Lorsqu'un fichier PHP est collé, son contenu est exécuté comme s'il existait physiquement à l'emplacement collé.

Plier les pages et insérer le contenu commun
---------------------------------------------

Souvent, nous devons créer plusieurs pages qui ont un contenu commun - par exemple, un menu.

En HTML simple, nous devrions d'abord créer une page avec un menu, puis la copier plusieurs fois. Mais en PHP, nous pouvons automatiser l'ensemble du processus.

Ayons un fichier `menu.html` où se trouve le contenu du menu et `index.php` où nous mettons le contenu et le menu.

Un exemple simple :

```php
<div class="page">
    <div class="contenu">
        <?php
            include __DIR__
            . '/article/' . ($_GET['page'] ?? 'Index') . '.html';
        ?>
    </div>
    <div class="menu">
                    include 'menu.html';
        ?>
    </div>
</div>
```

Ce script insère automatiquement le contenu de la page depuis le répertoire `/article` et lit le nom du fichier en fonction de la saisie de l'utilisateur (paramètre URL `?page=...`). Si aucun paramètre n'est passé, `index.html` est utilisé.

Ainsi, l'URL pourrait ressembler, par exemple, à `exemple.com?page=contacts` et charger `/article/contacts.html`.
