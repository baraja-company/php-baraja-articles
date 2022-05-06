File_get_contents
=================

> id: '6c8889f1-95e7-4540-9c3a-0225c6383954'
> slug:
> 	cs: file-get-contents
> 	en: file-get-contents
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: f8b180a5f7208dab0d37761a5acdc9e3

The **file_get_contents** function is used to read a file and put its contents into a variable. This function is similar to the <a href="/include">include</a> function, but unlike include, it can retrieve remote files on the Internet and transfer their contents via variables.

Sample
------

Either function can be used to load a local file from disk:

```php
$news = file_get_contents('news.html');

echo 'Current news:<br>' . $news;
```

Or from a remote URL:

```php
$page = file_get_contents('https://www.google.com');

echo $page;
```

When retrieving a URL, we can download any address and get its contents as a string into a variable. In the case of HTML, this is the source code.

The page is rendered incorrectly
----------------------------

This is because the HTML code is passed exactly as it is placed on the URL.

If the path to the image is for example `<img src="kocka.png">`, then this file may not exist in the context of our server, so we need to correct the path to for example: `<img src="https://server.cz/kocka.png">`.
