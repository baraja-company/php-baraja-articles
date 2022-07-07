PHP和服务器配置信息（phpinfo(), php.ini）。
================================

> id: '6f4a78c1-ca06-4619-9a28-a716a65a12a8'
> slug:
> 	cs: info
> 	zh: php-he-fu-wu-qi-pei-zhi-xin-xi-phpinfo-php.ini
> 
> perex:
> 	- 'PHP info, informace o nastavení a konfiguraci webového serveru. Změna konfigurace přes php.ini'
> 	- PHP信息，有关Web服务器设置和配置的信息。通过php.ini更改配置
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '9ad0961571b298fb1f30ea3e67b2973e'

我们经常需要找出尽可能多的关于服务器的信息，原生的`phpinfo()`函数在这方面非常好。

```php
phpinfo();

die;	// 写完配置后，退出脚本
```

这使得它很容易看到安装的版本、扩展、库和更多。

> 关于配置和改变设置的信息，见本文末尾。

找到一个特定的配置部分
-------------------------------------

有时，只列出特定的信息是很有用的，所以我们可以设置第一个参数，准确地指定我们感兴趣的内容。

```php
phpinfo(INFO_MODULES);
```

预先定义的常数用于设置。

| 常数名称 | 值 | 描述
|-------------------|-----------|------
| INFO_GENERAL | 1 | 一般配置、php.ini位置、最后更新日期、Web服务器、系统信息等。
| INFO_CREDITS | 2 | PHP积分，更多信息见`phpcredits()`。
| INFO_CONFIGURATION| 4 | 当前位置和设置指令。更多信息将由`ini_get()`函数提供。
| INFO_MODULES | 8 | 关于已安装模块的信息。欲了解更多信息，请参见`get_loaded_extensions()`函数。
| INFO_ENVIRONMENT | 16 | 关于 "环境 "变量的信息，可作为"$_ENV"。
| INFO_VARIABLES | 32 | 超全局变量设置概览，被称为`EGPCS'（环境、GET、POST、Cookie、服务器）。
| INFO_LICENSE | 64 | 关于PHP使用许可和其他使用条款的信息。
| INFO_ALL | -1 | 显示所有信息（默认值）。

超全局变量`$_SERVER`。
---------------------------------

我们可以在运行时直接找出相当多的关于服务器设置的信息（例如，给网站管理员的电子邮件，访问者的当前IP地址，或当前调用的URL）。

列出所有现有的价值是很容易的。

```php
foreach ($_SERVER as $key => $value) {
    echo $key . ':' . $value . '<br>';
}
```

> **警告：**并非所有的索引都需要存在（例如，如果脚本在CLI模式下运行cron，带有页面URL或请求的IP地址的索引将不存在）。

读取特定的配置指令
-----------------------------------------

大部分配置都保存在 `php.ini`中，不能以正常方式从 PHP 中直接访问。例如，最大的上传文件大小。

要直接读取配置，请使用`ini_get()`函数（注意：这个函数不一定在所有的服务器上都启用，对主机来说更是如此）。

例如，如果我们想找出我们可以上传的最大文件大小，我们必须写出我们自己的实现。

```php
/**
 * @作者Jan Barášek
 */
public static function getMaxUploadFileSize(): int
{
    $maxUpload = min(
        ini_get('帖子最大尺寸'),
        ini_get('上传_最大文件尺寸')
    );

    if (strncmp($maxUpload, 'M', 1) === 0) {
        return (int) str_replace('M', '', $maxUpload);
    }

    return (int) $maxUpload;
}
```

返回`upload_max_filesize`和`post_max_size`的最大值（MB）。

配置服务器和改变设置
-------------------------------------

这些设置本身存储在`php.ini`文件中。它的位置可以通过使用`phpinfo()`函数或调用`php -ini`命令轻松找到。

```shell
> php --ini

Configuration File (php.ini) Path: /etc/php/7.1/cli
Loaded Configuration File:         /etc/php/7.1/cli/php.ini
Scan for additional .ini files in: /etc/php/7.1/cli/conf.d
Additional .ini files parsed:      /etc/php/7.1/cli/conf.d/10-mysqlnd.ini,
/etc/php/7.1/cli/conf.d/10-opcache.ini,
/etc/php/7.1/cli/conf.d/10-pdo.ini,
/etc/php/7.1/cli/conf.d/20-calendar.ini,
/etc/php/7.1/cli/conf.d/20-ctype.ini,
/etc/php/7.1/cli/conf.d/20-exif.ini,
/etc/php/7.1/cli/conf.d/20-fileinfo.ini,
/etc/php/7.1/cli/conf.d/20-ftp.ini,
/etc/php/7.1/cli/conf.d/20-gd.ini,
/etc/php/7.1/cli/conf.d/20-gettext.ini
```

另外，路径可以被巧妙地解析（在Linux系统上适用）。

```shell
php -r "phpinfo();" | grep php.ini
```

他将会回来。

```shell
Configuration File (php.ini) Path => /etc/php/7.1/cli
Loaded Configuration File => /etc/php/7.1/cli/php.ini
```

> **重要的是：**通常根据环境和软件包将配置分为几个文件，其中`php.ini`对所有文件都有效，而例如`CLI`配置只对CLI模式有效，即从终端调用cron或命令。

配置上传文件大小限制
----------------------------------------------

一个经常直接在`php.ini`中配置的属性的例子是最大上传文件大小（默认是`2 MB`，这在2018年已经很低了）。

在配置文件内，例如，这写成如下。

```shell
; Maximum allowed size for uploaded files.
upload_max_filesize = 40M

; Must be greater than or equal to upload_max_filesize
post_max_size = 40M
```

*中点是指注释，后面是具体的配置指令。
