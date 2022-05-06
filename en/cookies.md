Cookies in PHP
==============

> id: '392dd88b-d2f5-4943-a993-01aaad7ccd32'
> slug:
> 	cs: cookies
> 	en: cookies-in-php
> 
> perex: 'Cookies je krátká textová informace v paměti prohlížeče, kam můžeme v PHP zapisovat a označit si uživatele.'
> publicationDate: '2019-09-11 10:18:29'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '78f9aad0278acd51b98179e7d29fe3d8'

> **Warning:** This article was written many years ago and some information may be outdated or incorrect. Please bear this in mind when reading.

Cookies are small pieces of textual information stored in a website visitor's browser. They are always transferred with every page that is loaded again, and can be deleted, changed and read by the user at any time, so they are not well suited to storing personal information.

*Warning: if your website uses cookies to track users or third-party add-ons (e.g. Facebook like button, Google analytics traffic meter, banner ads), you must inform the user about this.*

> "One more note: your site should not contain advertising or measurement codes until you have obtained consent. And that sucks."
>
> -- <a href="https://phpfashion.com/jak-na-souhlas-s-cookie-ve-zkurvene-eu">David Grudl</a>

Read a value from a cookie
--------------------------

All cookies are stored in the superglobal variable `$_COOKIE`, which stores each key as an array.

For example, if we have stored the name of the currently logged in user under the `user` key in the cookie, we can retrieve it easily:

```php
echo $_COOKIE['user'];
```

Note: Cookies may not always exist (for example, if you are a new user). Therefore, we should always check for the existence before any listing and offer an alternative error message if necessary.

```php
if (isset($_COOKIE['user']) && $_COOKIE['user']) {
    echo 'Logged in user: ' . $_COOKIE['user'];
} else {
    echo 'No one is logged in.';
}
```

Getting all available cookies
--------------------------------

Since all cookies are stored in the superglobal variable `$_COOKIE`, it is easy to list them:

```php
var_dump($_COOKIE);
```

Or alternatively, loop through and get all the keys and values:

```php
foreach($_COOKIE as $key => $value) {
    echo $key . ': ' . $value; // print out the key and value
    echo '<br>'; // wrap the line
}
```

Storing the value in the cookie
--------------------------

The `setcookie()` function is used to store data in cookies.

The first parameter is the cookie key, which is used to read it from the `$_COOKIE` field, and the second parameter is the data itself as a string.

With the third parameter we can (optionally) set the validity period for which the cookie will be available. The time of availability is given as <a href="/date">timestamp</a>, so if we want to set a cookie with a validity of 1 hour from this moment, we just need to write `time() + 3600`.

```php
$data = 'Some content we want to save.';

setcookie('TestCookie', $data);
setcookie('TestCookie', $data, time() + 3600);
```

Storing larger data
-------------------

Cookies are not suitable for storing larger data (browsers usually allow only 4 kB and a maximum of 20 cookies, the size also includes cookie names, validity settings, etc.).

It is better to store larger data on the server and just put an identifier in the cookie, by which we can tell which user it belongs to. This method is called `$_SESSION` and is discussed in a separate article.

If you don't necessarily need to store data always synchronous on the server, you can use the **<a href="https://jecas.cz/localstorage">localstorage</a>** storage available in javascript. Its capacity is in the order of MB and the data is not subject to expiration.
