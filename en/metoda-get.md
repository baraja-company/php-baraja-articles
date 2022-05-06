Getting parameters from URL by GET method
=========================================

> id: bbf2cb2c-f7f7-4be9-a9cd-960014db0f51
> slug:
> 	cs: metoda-get
> 	en: getting-parameters-from-url-by-get-method
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: caebe20b6b7cdf0b3703ff55dad6f094

You know, you have a page open, you follow the URL and you see a question mark with some parameters. An inexperienced programmer would think that these are separate files, but lo and behold. Try to create a file that has a question mark in its name (it doesn't work). **This is the reason why this article was written**.

What is it?
--------------------------

Actually, the point is that it's a single file that you pass variables to via a URL, so I have, say, a **index.php** file, and I pass the article name to it: **index.php?clanek=o-php**.

Code + explanation
--------------------------

Superglobal variable `$_GET` contains keys with parameters from URL

```php
echo $_GET['clanek'] ?? '';
```

Safety and length limits
--------------------------

The GET method is not secure, so confidential data should not be sent over it, one of the main reasons is that it is an unencrypted communication and secondly it is stored in history.

Confidential data or just everything should be sent using the <a href="/method-post">POST</a> method. GET is more suited for furmulars where it's good to show parameters (like search engines, article page) so that the page can be linked to.

The length of the GET is not unlimited! A lot of beginners pay for this. The maximum length is around 1024 characters (some places say 1088). So for longer texts, send <a href="/method-post">POST</a> with.
