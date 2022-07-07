在PHP中发送电子邮件(mail()和SMTP函数)
==========================

> id: '3051b5ea-9408-4aaa-8209-e3c527ef8ad2'
> slug:
> 	cs: odesilani-emailu-mail-smtp
> 	zh: zaiphp-zhong-fa-song-dian-zi-you-jian-mail-hesmtp-han-shu
> 
> perex:
> 	- 'Možnosti odesílání e-mailů v PHP, funkce mail(), SMTP, hlavičky, konfigurace a Nette Mailer.'
> 	- 'PHP中的电子邮件发送选项、mail()、SMTP、头信息、配置和Nette Mailer。'
> 
> publicationDate: '2019-11-26 12:00:02'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '53dc8075e16053ea9284299987c64952'

在PHP中，我们基本上有两种方法来发送邮件。

- 本地的`mail()`函数，有相当多的限制。
- 或通过一个SMTP服务器。

`mail()`函数必须使用SMTP服务器，这是一个非常简单的方法，通过SMTP服务器发送邮件。
---------------

使用这个方法的想法很简单：你调用这个函数。

```php
mail('jan@barasek.com', '主题', '信息的文本...');
```

而PHP会自己进行发送。

在内部，发送工作是通过读取`php.ini`中的配置，并寻找默认的SMTP服务器来发送邮件。所以这需要事先对网络服务器进行配置。

`mail()`函数的主要缺陷是，程序员必须自己弄清所有的逻辑。这涉及到，例如，扔掉关于加密的标题，将证书链接到加密信息，等等。

在发送失败的情况下，会返回一个 "false "值，我们必须自己捕捉和处理。我们可以通过调用`error_get_last()`有限度地找出具体的错误，因此，例如。

```php
if (@mail($to, $subject, $message) === false) {
	throw new \Exception(
		'不能发送邮件。'
		. (@error_get_last()['信息'] ?? '')
	);
}
```

> **TIP:** 注意，我们没有指定我们要发送邮件的地址和要使用的编码。
>
> 所有这些设置都需要通过头文件传递。

如果你仍然需要使用`mail()`函数（例如，因为托管），我建议使用`nette/mail`包和`SendmailMailer`服务，它可以很好地处理邮件的发送。

SMTP服务器
-----------

SMTP是 "简单邮件传输协议 "的缩写，（你很快就会看到）这一点非常正确。

SMTP与`mail()`不同，是一个更高级的协议，不仅有来自PHP方面的高级配置选项，也有直接在邮件服务器上的配置。

2018年，主机上的SMTP支持是非常好的。

SMTP基本上是由PHP首先建立一个与SMTP服务器的连接（它需要PHP中的`php_openssl.dll`扩展，你可能已经激活了），在连接过程中进行认证（验证登录凭证的正确性），然后我们可以以与数据库类似的方式与服务器通信--即发送单个请求，但始终保持一个连接。SMTP的一大优势是直接支持加密（称为 "TLS"）。

从本地主机发送电子邮件--一个简单的解决方案
--------------------------------------------------

当我在测试一个新写的应用程序时，我经常需要从localhost发送电子邮件。

> **为提示：**
>
> 在Mac上，情况很简单，因为MAMP服务器以某种方式 "神奇地 "找到了当前登录的Apple Mail账户，并且邮件总是从当前账户发送。

然而，你不能总是依赖这种行为，建立你自己的解决方案是一个好主意。如果你有互联网连接和谷歌账户，使用Gmail账户非常容易，你可以直接从PHP连接到Gmail，并通过它发送邮件。

如果你使用`nette/mail`包，配置很简单。

```neon
mail:
	smtp: true
	host: smtp.gmail.com
	username: janbarasek@gmail.com
	password: *********
	secure: ssl
```

> 该密码不是你的账户的登录密码（那将是不安全的，你不能使用**双因素认证，例如）。
>
> 你需要使用所谓的 "应用程序密码"，这在实现上意味着你<a href="https://myaccount.google.com/apppasswords">直接在你的谷歌账户中注册你的应用程序</a>，它被分配了一些随机生成的密码，你输入到PHP中，可以通过它来发送。
>
> 详细说明见<a href="https://support.google.com/accounts/answer/185833?hl=cs">谷歌网站上的</a>。

在Wedos上配置邮件
---------------------------

通过Wedos主机，你每天只能发送500封邮件，我在SMTP连接上挣扎了好一阵子。

通过`nette/mail`包，它是这样做的（一个可行的解决方案）。

```neon
mail:
	smtp: true
	host: smtp-*******.wedos.net
	username: jan@barasek.com
	password: ******
	secure: tls
	port: 587
```

每个主机的 "host "参数是不同的，可以在Wedos注册主机时发送的电子邮件中找到。

用户名 "代表邮件将被发送的邮箱。该邮箱必须存在。在PHP中发送邮件时，我们还需要设置发送到相同的地址（在Nette中使用`->setFrom()`方法）。

如果我们没有准确无误地填写配置，各种错误信息将被抛出，邮件将无法被发送。

当发送的信息数量超过时，将抛出一个异常，告知超过限制的情况。
