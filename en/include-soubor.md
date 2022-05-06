Include (folding pages from pieces)
===================================

> id: '4984832e-c11f-4e9e-8d3b-60561685389d'
> slug:
> 	cs: include-soubor
> 	en: include-folding-pages-from-pieces
> 
> publicationDate: '2019-08-23 15:06:33'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: aea5867cf788e623dddd3ced4ea24a7a

PHP is originally a templating language, which was created to make it easy to put pieces of pages together.

Supported formats
-------------------

Folding works as text, so it is advisable to use relevant formats such as `.html` or `.md`.

When a PHP file is pasted, its contents are executed as if they physically existed at the pasted location.

Folding pages and inserting common content
---------------------------------------------

Often we need to create several pages that have common content - for example, a menu.

In plain HTML, we would first create a page with a menu and then copy it many times. But in PHP we can automate the whole process.

Let's have a file `menu.html` where the menu content is and `index.php` where we put the content and the menu.

A simple example:

```php
<div class="page">
    <div class="content">
        <?php
            include __DIR__
            '/article/' . ($_GET['page'] ?? 'index') . '.html';
        ?>
    </div>
    <div class="menu">
        <?php
            include 'menu.html';
        ?>
    </div>
</div>
```

This script automatically inserts the page content from the `/article` directory and reads the file name according to the user input (URL parameter `?page=...`). If no parameter was passed, `index.html` is used.

So the URL might look like, for example, `example.com?page=contacts` and load `/article/contacts.html`.
