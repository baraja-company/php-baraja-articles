PHP Include - inserting a file into a page
==========================================

> id: '7a53145c-8552-425d-b864-283f73a7a7de'
> slug:
> 	cs: include
> 	en: php-include---inserting-a-file-into-a-page
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '6e18e67ec03b3a7592473230559fc364'

The `include` construct automatically inserts additional files/scripts into the current page.

As far as PHP is concerned, it will be automatically executed and understood as if it had always been at that location.

Example:

```php
include 'news.html';
```

Practical use
-----------------

- Common menus and menus,
- News, updates, news and the same content,
- Header or footer, ...

Dynamic file loading
--------------------------

We often need to load files dynamically, for example based on a variable.

For example:

```php
$clanek = 'neco';

include $clanek . '.html';
```

Or we can dynamically load articles into the page:

```php
include 'articles/' . $_GET['page'] . '.html';
```

Calling a URL with the `page` parameter will automatically insert the article, for example: `https://example.com/?page=novinky`.
