PHP function mail()
===================

> id: '6536e2e2-38e1-4496-80ef-6c1c9a0b5be5'
> slug:
> 	cs: odesilani-emailu
> 	en: php-function-mail
> 
> publicationDate: '2019-09-16 08:51:41'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: ca90a7366754a7e7a0ce43f4252296f8

The `mail()` function sends an e-mail message through the default server configuration. To work correctly, the function needs to be enabled on the server and the mail server needs to be configured for sending.

**The function is for sending only. You have to sort out receiving messages at the mail server level. For example, download messages regularly using `IMAP` or `POP3` protocol.**

> I strongly discourage the use of the function at the moment, because the programmer has to take care of everything himself (for example, send the correct headers, or set the encoding).
>
> Much better is to connect via <a href="/send-email-mail-smtp">SMTP server</a>.

Using
-------

```php
mail ('jan@barasek.com', 'Subject', 'Email text... ');
```

The first parameter gives the recipient's address, the second the subject and the third the message text. The fourth (optional) parameter gives the additional configuration of the message.
