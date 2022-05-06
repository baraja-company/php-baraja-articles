Superglobal variables
=====================

> id: bc2107ee-6551-4559-8c02-9cecdf98d33b
> slug:
> 	cs: superglobalni-promenna
> 	en: superglobal-variables
> 
> publicationDate: '2019-11-01 09:29:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '792e217f1331f38ed0719d7abeb05e67'

Superglobal variables are used to pass global application state and HTTP communication.

The main advantage of these variables is that they are always and everywhere available. In practice, they are arrays of values where we access specific information by index. In different contexts, the availability of keys may vary (explained below).

Types of superglobal variables
--------------------------------

All superglobals in PHP are arrays and are denoted by a dollar sign followed by an underscore (except `$GLOBALS`) and uppercase characters.

In `PHP 7` there are in particular the following:

| Variable | Description |
|-------------|-------|
| `$_GET` | URL parameters <a href="/methods-odesilani-dat">sent by the GET method</a>
| `$_POST` | Form data <a href="/methods-odesilani-dat">sent by POST</a>. Note that <a href="/ajax-post">may behave differently in ajax</a>.
| `$_REQUEST` | Form data sent by any method (`$_GET`, `$_POST` and `$_REQUEST`).
| `$_FILES` | Technical information about the currently uploaded files, for example via the `<input type="file">` construct
| `$_SERVER` | <a href="/info">Web server settings</a>, IP address, configuration... it varies depending on the environment (when calling a PHP script from Terminal it will contain different values and for example information about the current request will be missing).
| `$_COOKIE` | Configured <a href="/cookies">cookies</a>.
| `$_SESSION` | Session data (<a href="/sessions">session</a>), if it exists and has been set in the past.
| `$GLOBALS` | **Warning, it does not contain an underscore in the name!** This is the so-called <a href="global-variable">global-variable</a> and an alternative notation for the keyword `global`. If you have a global variable `$variable` in your application, you can also access it with the `$GLOBALS["variable"]` construct. However, using global variables is a bad and impure solution by design, so you'd better not do it.
| `$_ENV` | Information about the current environment where PHP is running.

Listing all existing values is easy to do:

```php
foreach ($_SERVER as $key => $value {
	echo $key . ': ' . $value . '<br>';
}
```

> Note: Not all indexes must always exist (for example, if the script runs cron in CLI mode, the index with the page URL or the IP address of the request will not exist).

Access to variables
-------------------

I recommend that all global variables (except `$_SESSION`) are read-only. This is because they contain global application data and other code may take this into account (for example, another installed library).

Another disadvantage of global state is that you can't always rely on exact values, even if they exist, so you should always check their keys with the `isset()` construct.

To save a new cookie, use `setcookie()` and do not insert the value directly. This is because it is read-only.

Lessons learned
-------

Never blindly trust the values of superglobal variables!

The user can use the URL and the headers sent to influence how the values are set. All input should always be carefully validated.

Register globals - the trouble with the old version of PHP
------------------------------------------

In the old version of PHP (up to `5.4.0`), there was a special `register-globals` directive (configurable in `php.ini`) that caused all passed parameters in a URL to be automatically registered as variables.

For example:

A user arrived at the URL: `https://example.com/script.php?var=24`

And PHP automatically created a variable `$var` with the value `24` within the script.

So it worked classically:

```php
<?php

echo $var;
```

So anyone could slip any variable into the script and change its contents. Obviously, security was not always a priority. Notquite.

Other sources
------------

For a more detailed description, see the <a href="https://www.php.net/manual/en/language.variables.superglobals.php">official manual</a>.
