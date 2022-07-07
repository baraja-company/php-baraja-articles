PHP函数mail()
===========

> id: '6536e2e2-38e1-4496-80ef-6c1c9a0b5be5'
> slug:
> 	cs: odesilani-emailu
> 	zh: php-han-shumail
> 
> publicationDate: '2019-09-16 08:51:41'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: ca90a7366754a7e7a0ce43f4252296f8

`mail()`函数通过默认的服务器配置发送一条电子邮件。为了正确工作，你需要在服务器上启用该功能，并设置邮件服务器进行发送。

**该功能仅用于发送。你必须在邮件服务器层面上对接收信息进行分类。例如，使用`IMAP`或`POP3`协议定期下载信息。

> 目前，我强烈反对使用该函数，因为程序员必须自己处理所有的事情（例如，发送正确的头信息，或设置编码）。
>
> 更好的是通过<a href="/send-email-mail-smtp">SMTP服务器</a>进行连接。

使用
-------

```php
mail ('jan@barasek.com', '主题', '电子邮件文本...');
```

第一个参数是收件人地址，第二个是主题，第三个是信息文本。第四个（可选）参数表示消息的附加配置。
