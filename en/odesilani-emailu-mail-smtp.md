Sending emails (mail() and SMTP functions) in PHP
=================================================

> id: '3051b5ea-9408-4aaa-8209-e3c527ef8ad2'
> slug:
> 	cs: odesilani-emailu-mail-smtp
> 	en: sending-emails-mail-and-smtp-functions-in-php
> 
> perex: 'Možnosti odesílání e-mailů v PHP, funkce mail(), SMTP, hlavičky, konfigurace a Nette Mailer.'
> publicationDate: '2019-11-26 12:00:02'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '53dc8075e16053ea9284299987c64952'

In PHP, we basically have 2 ways to send mails:

- The native `mail()` function, which has quite a few limitations,
- or via an SMTP server.

The `mail()` function has to use the SMTP server, which is a very simple way of sending mail through the SMTP server.
---------------

The idea of using this is simple: you call the function:

```php
mail('jan@barasek.com', 'Subject', 'Message text...');
```

And PHP will do the sending itself.

Internally, sending works by loading the configuration from `php.ini` and looking for the default SMTP server to deliver the mail through. So it requires the web server to be configured first.

The main pitfall of the `mail()` function is that the programmer has to figure out all the logic himself. This involves, for example, throwing away headers about encryption, linking certificates to encrypt messages, and so on.

In case of a send failure, the return value is `false`, which we have to catch and process ourselves. We can find out the specific error in a limited way by calling `error_get_last()`, so for example:

```php
if (@mail($to, $subject, $message) === false) {
	throw new \Exception(
		'Can't send mail: '
		. (@error_get_last()['message'] ?? '')
	);
}
```

> **HINT:** Note that we have not passed the address from which we want to send the mail and what encoding to use.
>
> All of these settings need to be passed via headers.

If you still need to use the `mail()` function (for example, because of hosting), I recommend using the `nette/mail` package and the `SendmailMailer` service, which handles the sending of mail well.

SMTP server
-----------

SMTP stands for `Simple Mail Transfer Protocol`, which (as you will soon see) is very true.

SMTP, unlike `mail()`, is a more advanced protocol with advanced configuration options not only from the PHP side, but also directly on the mail server.

SMTP support on hosts is excellent in 2018.

SMTP basically works by PHP first establishing a connection to the SMTP server (it requires the `php_openssl.dll` extension in PHP, which you probably already have active), authenticating (verifying that the login credentials are correct) during the connection, and then we can communicate with the server in a similar way as with a database - i.e. send individual requests, but keep a single connection at all times. A big advantage of SMTP is the direct support for encryption (known as `TLS`).

Sending emails from localhost - a simple solution
--------------------------------------------------

I often need to send emails from localhost when I am testing a newly written application.

> **For tip:**
>
> On a Mac, the situation is simple because the MAMP server somehow "magically" finds the currently logged in Apple Mail account and messages are always sent from the current account.

However, you can't always rely on this behaviour and it's a good idea to set up your own solution. If you have an internet connection and a Google account, it is very easy to use a Gmail account that you can connect to directly from PHP and send mail through it.

If you use the `nette/mail` package, the configuration is simple:

``neon
mail:
	smtp: true
	host: smtp.gmail.com
	username: janbarasek@gmail.com
	password: *********
	secure: ssl
```

> The password is not the login password for your account (that would be insecure and you couldn't use **two-factor authentication**, for example).
>
> You need to use what is called an "application password", which implementationally means that you <a href="https://myaccount.google.com/apppasswords">register your application</a> directly in your Google account, which is assigned some randomly generated password that you enter into PHP and can be sent through.
>
> Detailed instructions are <a href="https://support.google.com/accounts/answer/185833?hl=cs">on Google's website</a>.

Configuring mail on Wedos
---------------------------

You can only send 500 emails per day through Wedos hosting, and I struggled with the SMTP connection for a while.

Through the `nette/mail` package it is done like this (a workable solution):

``neon
mail:
	smtp: true
	host: smtp-*******.wedos.net
	username: jan@barasek.com
	password: ******
	secure: tls
	port: 587
```

The `host` parameter is different for each hosting and can be found in the email Wedos sends when you register the hosting.

The `username` represents the mailbox from which the mails will be sent. The mailbox must exist. When sending mail in PHP, we also need to set the send to the same address (in Nette with the `->setFrom()` method).

If we don't fill out the configuration accurately and correctly, various error messages will be thrown and the mails will not be able to be sent.

When the number of sent messages is exceeded, an exception will be thrown informing about exceeding the limit.
