PHP and server configuration information (phpinfo(), php.ini)
=============================================================

> id: '6f4a78c1-ca06-4619-9a28-a716a65a12a8'
> slug:
> 	cs: info
> 	en: php-and-server-configuration-information-phpinfo-php.ini
> 
> perex: 'PHP info, informace o nastavení a konfiguraci webového serveru. Změna konfigurace přes php.ini'
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '9ad0961571b298fb1f30ea3e67b2973e'

Often we need to find out as much information about the server as possible, the native `phpinfo()` function is great for this:

```php
phpinfo();

die; // after listing the configuration, exit the script
```

This makes it easy to find out the installed version, extensions, libraries and much more.

> See the end of this article for information about configuration and changing settings.

Finding a specific configuration section
-------------------------------------

Sometimes it is useful to list only specific information, so we can set the first parameter to specify exactly what we are interested in:

```php
phpinfo(INFO_MODULES);
```

Predefined constants are used for setting:

| Constant Name | Value | Description
|-------------------|-----------|------
| INFO_GENERAL | 1 | General configuration, php.ini location, last update date, Web Server, system information and more.
| INFO_CREDITS | 2 | PHP Credits, for more see `phpcredits()`.
| INFO_CONFIGURATION| 4 | Current location and settings directives. More information will be provided by the `ini_get()` function.
| INFO_MODULES | 8 | Information about installed modules. For more information, see the `get_loaded_extensions()` function.
| INFO_ENVIRONMENT | 16 | Information about the `Environment` variable, available as `$_ENV`.
| INFO_VARIABLES | 32 | An overview of superglobal variable settings, known as `EGPCS` (Environment, GET, POST, Cookie, Server).
| INFO_LICENSE | 64 | Information about PHP usage license and other terms of use.
| INFO_ALL | -1 | Display all information (default value)

Superglobal variable `$_SERVER`
---------------------------------

We can find out quite a lot of information about the server settings directly at runtime (for example, the email to the webmaster, the current IP address of the visitor, or the currently called URL).

Listing all existing values is easy:

```php
foreach ($_SERVER as $key => $value) {
    echo $key . ': ' . $value . '<br>';
}
```

> **Warning:** Not all indexes need to exist (for example, if the script runs cron in CLI mode, the index with the page URL or the IP address of the request will not exist).

Reading specific configuration directives
-----------------------------------------

Much of the configuration is stored in `php.ini` and is not directly accessible from PHP in the normal way. For example, the maximum upload file size.

To read the configuration directly, use the `ini_get()` function (note: this function may not be enabled on all servers, this is especially true for hosts).

For example, if we want to find out the maximum file size we can upload, we have to write our own implementation:

```php
/**
 * @author Jan Barášek
 */
public static function getMaxUploadFileSize(): int
{
    $maxUpload = min(
        ini_get('post_max_size'),
        ini_get('upload_max_filesize')
    );

    if (strncmp($maxUpload, 'M', 1) === 0) {
        return (int) str_replace('M', '', $maxUpload);
    }

    return (int) $maxUpload;
}
```

Returns the maximum value of `upload_max_filesize` and `post_max_size` in MB.

Configuring the server and changing settings
-------------------------------------

The settings themselves are stored in the `php.ini` file. Its location can be easily found by using the `phpinfo()` function or by calling the `php --ini` command.

``shell
> php --ini

Configuration File (php.ini) Path: /etc/php/7.1/cli
Loaded Configuration File:         /etc/php/7.1/cli/php.ini
Scan for additional .ini files in: /etc/php/7.1/cli/conf.d
Additional .ini files parsed: /etc/php/7.1/cli/conf.d/10-mysqlnd.ini,
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

Alternatively, the path can be handily parsed (works on Linux systems):

```shell
php -r "phpinfo();" | grep php.ini
```

Returns:

```shell
Configuration File (php.ini) Path => /etc/php/7.1/cli
Loaded Configuration File => /etc/php/7.1/cli/php.ini
```

> **Important:** Usually the configuration is split into multiple files based on environment and packages, with `php.ini` applying globally to all of them, while for example `CLI` configuration applies only to CLI mode, i.e. calling a cron or Terminal command.

Configuring the upload file size limit
----------------------------------------------

An example of a property that is often configured directly in `php.ini` is the maximum uploaded file size (the default is usually `2 MB`, which is already low in 2018).

Within the configuration file this is written as follows, for example:

```shell
; Maximum allowed size for uploaded files.
upload_max_filesize = 40M

; Must be greater than or equal to upload_max_filesize
post_max_size = 40M
```

*Midpoint indicates a comment, followed by specific configuration directives.*
