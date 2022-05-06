Sessions - server cookies in PHP
================================

> id: ab782ff0-d5ba-4115-9ebb-ab37978a6527
> slug:
> 	cs: sessions
> 	en: sessions---server-cookies-in-php
> 
> publicationDate: '2019-11-06 17:06:18'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '3c84a09e5d180f6a47a58bea70b7ab21'

Often we need to store more information in <a href="/cookies">cookies</a>, but the maximum limit for cookies is 4 kB, which is not much. Sessions solves this problem by storing the data on the web server, and only stores a short identifier in the client's browser to tell which data belongs to which client.

Starting a session
---------------------

Before doing any work with sessions, we must first start them. This is done by calling the `session_start()` function right at the beginning of the script:

```php
session_start();
```

> Strong warning: no output to HTML code must be executed before calling the `session_start()` function!

Session security
-------------------

The content of the session is stored on the server and only the identifier is sent to the client browser, so the user has no way of knowing what is stored in the session. The only way the script can affect the user is by deleting the identifier (whereupon the script will generate a new one).

Getting data from a session
----------------------

All sessions are stored in the superglobal variable `$_SESSION` and can be traversed as an array.

For example, the name of the currently logged in user can be retrieved by writing:

```php
echo $_SESSION['user'];
```

Note: Session may not always exist (for example, if it is a new user). Therefore, before any listing, we should always check for existence and offer an alternative error message if necessary.

```php
if (isset($_SESSION['user']) && $_SESSION['user']) {
    echo 'Logged in user: ' . $_SESSION['user'];
} else {
    echo 'No one is logged in.';
}
```

Saving data to the session
----------------------

Saving is done as a simple saving of data into a variable:

```php
$_SESSION['user'] = 'Honzík';
```

The web server will take care of the technical aspects of storing the identifier correctly on the server and sending it to the user.

Deleting sessions
----------------

We can delete individual values separately according to the key:

```php
unset($_SESSION['user']);
```

Or alternatively, all available sessions:

```php
unset($_SESSION);
```

> Note: Deleting a particular session does not empty the key value, but it does completely delete the key. Therefore, an error warning will be thrown when attempting to read a non-existent key. The existence of a key can always be easily verified with the `isset()` function.

Maximum session validity
---------------------------------

Each saved session has a limit on how long it will be stored on the server. PHP directly contains a cron script that periodically deletes old sessions.

The default is usually `1440 seconds`, which is `24 minutes`.

Increasing the value needs to be done in 2 places:

- In <a href="/info">`php.ini` the maximum length of validity the server will maintain is set</a>. The value is set by the `session.gc_maxlifetime` directive,
- On the PHP script side, you need to specify the current requested validity.

Usage in PHP:

```php
// the server will now hold the session with a validity of up to 3600 seconds = 1 hour
ini_set('session.gc_maxlifetime', '3600');

// all clients (browsers) will be
// a session with a validity of exactly 3600 seconds will be sent
session_set_cookie_params(3600);

session_start(); // we can start the session!
```
