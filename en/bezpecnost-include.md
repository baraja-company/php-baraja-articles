Include security in PHP and file attachment
===========================================

> id: '820f8de6-ff1e-406c-8fe5-95080642656f'
> slug:
> 	cs: bezpecnost-include
> 	en: include-security-in-php-and-file-attachment
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1207d637c20fcb5e8f609be2eb449135'

Often, we may want to attach a file to a page that we have stored on disk somewhere else. If we enter its exact name directly into the attach function, there is nothing to worry about.

Securely attaching a file
--------------------------

```php
include 'menu.html';
```


The previous entry is perfectly safe, because we always attach the same file. A security error cannot occur in this case. The only problem that can occur is the absence of the **menu.html** file, which will trigger a warning message (which probably won't appear anyway), but this is rare because we usually attach a file whose existence is virtually certain.

Attaching a file according to the pattern
--------------------------

But what if, for example, we want to attach articles to a simple content site like this one? On this site, I have a physical folder where the articles are stored in HTML format and I attach them directly to the source code.

However, simply connecting is not enough! A beginner can call individual articles like this:

```php
include 'articles/' . $_GET['clanek'] . '.html';
```

> But this is extremely dangerous. An attacker can pass a link to another directory using `../` or something similar as the article name, and sometimes it's possible to get rid of the ending by passing a null byte at the end. You should at least use the `basename()` function, but better to allow only whitelist values.

Why not load non-relevant files?
--------------------------

Often we don't mind loading an incorrect (unexpected) file - it's the user's fault for requesting a page they don't actually want, but there may be situations where it does matter. In particular:

- The user loads a file that the public cannot access and only the server can get to it.
- Loading some other PHP script may trigger an unexpected action or error message that can give a hint about how the site is working and help lead to further attacks.
- Loading another file may not only cause it to be added to the document, but also cause it to run.

Whitelist and input validation
--------------------------

If we don't have the ability to validate input in some secure way (e.g. from whitelist), we shouldn't rely on the honesty of the user and defend scripts at least at the PHP level anyway.

The first important thing is to put all loaded files in the same folder (directory) and disable some dangerous characters, especially slash and dot. This will make it impossible to access other folders that contain potentially vulnerable files. Disabling dangerous characters can also be done by simply deleting them from the input string.

```php
$load = '../index'; // this input could be potentially dangerous
$load = strtr($load, './', ''); // delete all periods and slashes from the string
include $load .'.html';
```


Running files?
--------------------------

It's important to note that the **include** construct executes the files when connected in the same way as if they were PHP code, so it's a good idea to allow for this possibility.

Often, however, we will be attaching files that are not required to be executed afterwards, and we are only interested in the stored text (content) in the form of a string. Therefore, we can load the file into a variable and work with it as a string, which is quite safe.

```php
$load = '../index'; // this input could be potentially dangerous
$load = strtr($load, './', ''); // deletes all periods and slashes from the string
$file = file_get_contents($load . '.html'); // load the contents into a variable
echo $file; // dump the contents of the file
```

This solution looks interesting and safe at first glance - and it is safe. Even if the user manages to call the PHP file, it will never run. However, it may display it (meaning its source code) and we need to be careful of that.

Recognizing the desired file from the script
--------------------------

There is no definitive guide for this, everyone has to do it themselves according to the needs of the script. For example, I recognize an article from other files by the fact that they contain a heading of size H1. So if someone loads a file where there is no heading, I don't display anything and the page ends with an error message. It's always important to find some unique feature that only the files you want have that the other files don't have, and you can go from there.

Conclusion
--------------------------

Although validating and loading files is relatively simple, a large number of beginners still make mistakes - and will continue to make mistakes. The most important thing is to get the meaning of what we are loading right and how to distinguish the content we want from the rest. And most importantly, work with the content as a string and never load it directly into the page.
