如何设置HTTPS/SSL证书--完整指南
=====================

> id: '0346a091-2b2c-494f-b2fb-573796d4fb46'
> slug:
> 	cs: jak-nastavit-https-ssl-certifikat
> 	zh: ru-he-she-zhihttps-ssl-zheng-shu-wan-zheng-zhi-nan
> 
> publicationDate: '2019-09-16 09:01:35'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '67b2b3ccce4815204a4bcadce781fa59'

在客户网站上部署`https'协议时，我经常遇到各种困难，这些困难源于对问题的不理解和概念的过于复杂。

在本教程中，我详细描述了在网络服务器上获取和部署有效证书的步骤。

**在每个小标题中，我总是为高级用户简要地总结该步骤，并在底部为初学者讨论细节。

> **警告：**部署证书的整个过程可能需要一个多小时，而且往往是间歇性的（网站可能无法使用）。

输入要求
-----------------

说明中假定我们可以访问一个在Linux上运行的使用Apache的终端网络服务器。

对于Nginx，整个理论同样适用，只是证书文件的链接方式不同。

##连接到网络服务器

我们通过SSH连接到服务器。

- 在Windows上，我推荐使用**Putty**程序。
- 在Mac或Linux上，只需使用内置的终端。

在Mac或Linux上，调用该命令。

```htaccess
ssh uživatel@server
```

例如，我想连接到`baraja.cz`网站上的用户`root`。

```htaccess
ssh root@baraja.cz
```

或者向用户`jan`发送一个特定的IP地址。

```htaccess
ssh jan@127.0.0.1
```

提交查询后，要么直接进行连接，要么要求你提供密码。输入密码时没有任何显示，所以用回车键确认密码，并等待连接授权。

> **警告：**如果我们对一个行动没有权利，我们需要分配它们。要么用`sudo su`命令直接切换到`root`用户，要么在我们想在root下执行的命令前面加上`root`字样，例如`root rm <name>`在root下将删除文件`<name>`。当使用`sudo`命令时，我们可能会定期被提示输入密码。

**详情：**

SSH访问是由你租用服务器的特定主机设置的。

- 在VPS的情况下，你将总是得到SSH访问。
- 在托管的情况下，你可能根本得不到SSH，而且配置是以不同的方式进行的（通常是通过网页界面，或联系支持）。

##从HTTP切换到HTTPS

如果你正在将一个现有的网站从`http`转换到`https`，你需要保证所有的流量都被重定向到新的`https`协议。

在Apache的情况下，这可以通过在`.htaccess`文件中使用一个重定向来轻松实现。

```htaccess
<IfModule mod_rewrite.c>
	RewriteEngine On

	RewriteCond %{HTTPS} !on
	RewriteRule .? https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>
```

这个配置将确保所有对`http`的请求将以`HTTP代码301`重定向到`https`。这个配置是Nette框架的默认配置，但也适用于所有其他情况。

**详情：**

.htaccess "文件包含影响每个请求的具体网络服务器配置。它通常与`index.php`放置在同一目录下，或其他可从互联网上访问的文件。

它的设置只对Apache服务器有效，对某些主机可能被禁用或限制。如需更详细的信息，请随时联系你的网站所在的主机。

-----

有时会发生`.htaccess`不愿意很好地合作，而且配置非常困难（例如对于许多域，每个域的行为都不同）。在这种情况下，重定向到HTTPS可以直接在PHP中处理。

```php
if (empty($_SERVER['HTTPS']) || $_SERVER['HTTPS'] === '关闭') {
	$location = 'https://' . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI'];
	header('HTTP/1.1 301永久移动');
	header('位置。' . preg_replace('/^(https:///(?:www\.)(.*)$/', '$1$2', $location));
	die;
}
```

将该脚本放在`index.php`中。然而，我并不推荐这种解决方案。

##查找具有虚拟主机的文件

在Apache的情况下，我们需要找到虚拟主机文件。

它们通常位于`/etc/apache2/sites-available`路径下。

用`ls -al`命令列出该目录的内容，并找到为我们的网站设置虚拟的文件。

虚拟主机通常位于扩展名为".conf "的文件中。默认值通常在`000-default.conf`中。

**详情：**

- 该目录用`cd`命令打开，例如`cd /etc/apache2/sites-available`。
- 文件的内容用`cat <name>`命令写入终端，或用`nano <name>`或`vim <name>`命令进行编辑。
- 通常通过使用`CTRL + X`键盘快捷键或按`ESC`两次来访问编辑器。
- 文件可以通过Midnight commander（`mc`命令）在GUI中进行部分浏览，在Ubuntu的情况下，可以通过`apt-get install mc`或`sudo apt-get install mc`简单安装。

##为443端口设置一个虚拟主机

在虚拟主机中，我们需要为端口`443`准备一个新的端口（一个常见的问题可能是防火墙上的端口443被阻挡）。

例如，该文件的内容可能看起来像这样。

```htaccess
<IfModule mod_ssl.c>
     <VirtualHost *:443>
         ServerName baraja.cz

         ServerAdmin jan@barasek.com
         DocumentRoot /var/www/baraja.cz/www
         Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains; preload"
         ErrorLog ${APACHE_LOG_DIR}/baraja.cz.ssl.error.log
         CustomLog ${APACHE_LOG_DIR}/baraja.cz.ssl.access.log combined
         SSLEngine on
         SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
         SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
         SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
         SSLProtocol             all -SSLv3
         SSLCipherSuite          ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS
         SSLHonorCipherOrder     on
         SSLCompression          off
         <FilesMatch "\.(cgi|shtml|phtml|php)$">
                 SSLOptions +StdEnvVars
         </FilesMatch>
         <Directory /usr/lib/cgi-bin>
                 SSLOptions +StdEnvVars
         </Directory>
		 BrowserMatch "MSIE [2-6]" nokeepalive ssl-unclean-shutdown downgrade-1.0 force-response-1.0
         # MSIE 7 and newer should be able to use keepalive
         BrowserMatch "MSIE [17-9]" \
         ssl-unclean-shutdown
     </VirtualHost>
</IfModule>
```

文件上是最重要的区域。

```htaccess
SSLEngine on
SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
```

了解这种配置是绝对关键的。如果你需要在谷歌上查找更详细的信息，请使用所有Apache服务器通用的`SSLCertificateFile`、`SSLCertificateKeyFile`和`SSLCertificateChainFile`。

> **警告：** SSL问题相对较老，为了保持向后的兼容性，同一事物通常使用不同的名称。因此，理解这一原则很重要。

**详情：**

重要的是，VirualHost包含。

- `<IfModule mod_ssl.c>`说我们要使用SSL
- `<VirtualHost *:443>`，所有通信将在443端口运行。
- `SSLEngine on`表示此虚拟主机已启用SSL。
- `SSLCertificateFile`、`SSLCertificateKeyFile`和`SSLCertificateChainFile`是具有特定密钥的文件。

不需要其他任何东西。

##了解我们要做的事情的原理和证书的工作原理

在VirtualHost的最终配置中，我们实际上只需要3个文件`SSLCertificateFile`、`SSLCertificateKeyFile`和`SSLCertificateChainFile`，我们可以把它们放在服务器的任何地方（名称和位置都不重要）。重要的是要提供一个工作路径，并且文件的内容是有效的。

获得证书的具体方法可能因CA而异。重要的是要理解原则，然后将其应用于你的案件。

| 文件 | 意义 |
|---------------------------|-------------------------------------|
| `SSLCertificateFile` | 此证书**是由当局发送的** |
| `SSLCertificateKeyFile` | **我生成的**私人密钥 |
| `SSLCertificateChainFile` | 我从网上下载了**中级+根** |

## 获取私钥并申请证书

在服务器上，我们首先生成私钥。**private**这个词很重要，它意味着除了网络服务器，没有其他人知道它。理想情况下，它应该永远不会离开服务器，并应放在一个安全的地方。失去这个密钥就意味着失去了安全，因为攻击者将能够冒充一个特定的服务器。

要生成密钥，请使用命令。

```shell
openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.key -out yourdomain.csr
```

为了生成它，我们需要在服务器上安装`openssl`程序，例如我们可以通过运行`sudo apt install openssl`获得。

可以有几种钥匙，在这种情况下，我们生成一个2048字节长的RSA钥匙（`rsa:2048`）。

命令的输出是2个文件（你根据你的域名来命名）。

- `yourdomain.key` - 这是私人密钥。将Apache VirtualHost中这个密钥的路径保存为`SSLCertificateKeyFile`。
- `yourdomain.csr` - 这是一个`证书请求'，或者说是一个签发证书的请求。

请求的内容必须始终提交给CA批准。这通常是通过出售证书的网站上的管理部门的网络界面进行的。对请求的批准根据证书的类型而有所不同。它最常由机器人自动完成，需要5分钟到8小时。就昂贵的证书而言，网站和运营公司的实际所有权也要得到核实，核实工作由人工完成，可能需要几天时间。

如果你很着急，有一个认证机构 "Let's encrypt "可以自动授权请求，有效期为3个月。

> **注意：**在某些情况下，CA提供自动请求生成。这不是一种推荐的做法，因为它知道私钥。如果你这样做，你必须始终从该机构获得私钥，并将其放在服务器上的一个文件中，其方式与你生成请求的方式相同。

##获得特定CA/安全机构的密钥

同时，在等待请求被批准和证书被签发的过程中，我们将确保CA的**公共密钥的安全。在Apache VirtualHost中，这由`SSLCertificateChainFile`文件表示（在英语中，这被称为`Intermediate and Root CA`）。这个密钥原则上是公开的，它告诉网络浏览器谁是CA。这个文件通常从CA的网站上下载，或者通过电子邮件传递给我们。

你应该根据你选择的算法，始终下载正确的密钥。以RapidSSL为例，它可以在这里下载：https://knowledge.digicert.com/generalinformation/INFO1548.html

##获得证书

根据该请求，证书颁发机构向我们颁发了一份证书。在Apache VirtualHost的情况下，它由`SSLCertificateFile`文件表示。

重要的是，证书的有效期是有限的，我们需要重复这个过程（最好是在过期之前）。在Chrome浏览器中，你可以通过点击浏览器中的绿色挂锁和查看证书的详细信息来轻松找到到期日，如果它是由CA传达的。

过期后，该证书无效，网站将拒绝连接，用户将收到关于安全漏洞的错误信息。

## 检查并进行修改

Apache设置的正确性可以通过`apache2ctl -S`命令进行部分验证。

为了使这些变化生效，必须重新启动Apache，例如用命令。

```shell
sudo service apache2 restart
```

如果没有抛出错误信息，我们立即通过网络浏览器去验证功能（我推荐谷歌浏览器，它显示的细节最多）。

在出现错误的情况下，我们调用`apache2ctl -S`命令，它可以显示日志的路径，在那里我们可以看到大约是什么问题。重要的是，要按照正确的顺序完成本手册中的所有步骤，不要调换任何键。

## 未来支持

不要忘记在你的日历上标记到期日期，以便你能及时更新你的证书。有些CA会发送通知邮件，但这并不总是可靠的。

你可以在当前证书运行的同时进行更新程序并获得新的证书，然后直接在同一时间更换。

对于自动更新，我推荐[Certbot](https://certbot.eff.org)，它可以自动监测有效性并更新证书。例如，更新是由一个cron完成的，它每月在晚上调用一个命令来签发一个新的证书，并立即部署它。

如果你不了解证书管理，在一家成熟的公司托管是一个好主意，该公司将为你提供功能性托管，包括一个证书。这将为你节省很多麻烦。
