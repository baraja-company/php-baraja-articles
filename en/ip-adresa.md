Getting the user's IP address in PHP
====================================

> id: '1d6d761e-c139-4624-8c32-b0f2c131a831'
> slug:
> 	cs: ip-adresa
> 	en: getting-the-user-s-ip-address-in-php
> 
> perex: 'Zjištění IP adresy uživatele v PHP, ukládání IP adresy a ban uživatele. Ošetření VPN a uživatele za Proxy nebo NATem.'
> publicationDate: '2020-02-28 10:30:21'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '54a77b5e4cc431b354903e42a27682a6'

In PHP, it is very easy to detect an IP address at a basic level:

```php
echo 'You know, your IP address is ' . $_SERVER['REMOTE_ADDR'] . '?';
```

> **Warning:** Getting the IP address as the key of the `$_SERVER['REMOTE_ADDR']` field is only possible if PHP was called from a browser. In CLI mode (for example, running from Terminal with cron), the IP address is not available (this makes sense, since no network request is being made).

Reliable IP address discovery
-----------------------------

After many years of development, I finally stuck with this implementation:

```php
function getIp(): string
{
    if (isset($_SERVER['HTTP_CF_CONNECTING_IP'])) { // Cloudflare support
        $ip = $_SERVER['HTTP_CF_CONNECTING_IP'];
    } elseif (isset($_SERVER['REMOTE_ADDR']) === true) {
        $ip = $_SERVER['REMOTE_ADDR'];
        if (preg_match('/^(?:127|10)\.0\.0\.[12]?\d{1,2}$/', $ip)) {
            if (isset($_SERVER['HTTP_X_REAL_IP']) {
                $ip = $_SERVER['HTTP_X_REAL_IP'];
            } elseif (isset($_SERVER['HTTP_X_FORWARDED_FOR'])) {
                $ip = $_SERVER['HTTP_X_FORWARDED_FOR'];
            }
        }
    } else {
        $ip = '127.0.0.1';
    }
    if (in_array($ip, ['::1', '0.0.0.0', 'localhost'], true) {
        $ip = '127.0.0.1';
    }
    $filter = filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_IPV4);
    if ($filter === false) {
        $ip = '127.0.0.1';
    }

    return $ip;
}
```

Much better then:

```php
echo 'Do you know that your IP address is ' . getIp() . '?';
```

If the IP can be found out directly, or is IPv6 only, or is CLI mode (for example, cron), `127.0.0.1` (localhost) is returned.

Implementations that take into account the `X-Forwarded-For` and `X-Real-IP` headers are extremely dangerous directly in PHP, because data can easily be modified and an attacker can spoof a fake IP address to, for example, view the administration or activate the Debug mode of the site (Nette Tracy). On the other hand, we have to accept some proxied requests (for example, when proxing traffic through Cloudflare, or when running Apache and Ngnix on the same machine, when they are called locally right after each other).

In the case of direct user access to the server, there is only one correct solution, and that is to ensure on Apache (via the `RemoteIP` extension) and in Nginx via the `remote_ip` extension that `X-Forwarded-For` is set from the actual IP address of the visitor, and that the IP address cannot be set with an HTTP header.

The `$_SERVER['REMOTE_ADDR']` field automatically gets the correct IP address (that is, the IP address from which the request came directly to PHP) and we don't have to deal with it.

User access via proxy
----------------------------

It often happens that a user accesses through a proxy. Then the actual IP address is stored in the variable `$_SERVER['HTTP_X_FORWARDED_FOR']`.

This case can occur, for example, when routing on the server is resolved using the `Ngnix -> Apache -> PHP` method, where `Ngnix` serves as a reverse proxy before `Apache`. In this case, PHP sees only the IP address within the internal network (usually of the form `127.0.0.*`).

For example, the **Cloudflare** service may behave in this way, and care should be taken whether we are working with the IP address of the actual user or the proxy. For me, the best way is to use the `getIp()` function mentioned at the beginning of this article. We can ensure Cloudflare detection by verifying the existence of the `$_SERVER['HTTP_CF_CONNECTING_IP']` key, which is automatically transmitted in every proxied request.

Storing the IP address
------------------

It depends on what IP address you have available.

- IPv4 IP address can be stored in 4 bytes, the `ip2long` function is used for this,
- However, for an IPv6 IP address we have to use 16 bytes and there is no conversion function.

If your database server does not directly support a data type for the IP address, I recommend storing the IP address as `varchar(39)`, where both versions will fit as a string and be human readable.

> When storing the IP address, consider whether it makes sense to also store the domain name detected by the `gethostbyaddr` function. When listing and searching, you can't find out the names because it takes a very long time and they can change over time.

Blocking the visitor's IP address
-----------------------------

The ideal solution is to create a list of blocked IP addresses and compare this list with the current IP address on each request. If the addresses match, the request will be stopped immediately.

```php
$blackList = [
    'first-ip',
    'second-ip',
];

if (\in_array(getIp(), $blackList, true) === true) {
    echo 'Unfortunately you have a blocked ip address :-(';
    die; // End request
}
```

The example assumes an implementation of `getIp()` as in the example above.

A more powerful solution is to verify the occurrence of the index in the array:

```php
$blackList = [
    'first-ip' => true,
    'second-ip' => true,
];

if (isset($blackList[getIp()]) === true) {
    echo 'Unfortunately you have a blocked ip address :-(';
    die; // End request
}
```

Server IP address and server name
---------------------------------

The IP address of the server is usually stored in the `$_SERVER['SERVER_ADDR']` field and its name can be obtained by the `gethostbyaddr($_SERVER['SERVER_ADDR'])` construct.

However, if the `Ngnix -> Apache -> PHP` concept is used and `Ngnix` is in the role of a reverse proxy, the real IP address of the server is not displayed.

In this case, the server name can be found in the `$_SERVER['SERVER_NAME']` field, or by using the `php_uname('n')` function. [Official uname function documentation](https://www.php.net/manual/en/function.php-uname.php).

We can then use this trick to find out the public IP address of the server: `gethostbyname(php_uname('n'))`.
