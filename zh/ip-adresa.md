在PHP中获取用户的IP地址
==============

> id: '1d6d761e-c139-4624-8c32-b0f2c131a831'
> slug:
> 	cs: ip-adresa
> 	zh: zaiphp-zhong-huo-qu-yong-hu-deip-de-zhi
> 
> perex:
> 	- 'Zjištění IP adresy uživatele v PHP, ukládání IP adresy a ban uživatele. Ošetření VPN a uživatele za Proxy nebo NATem.'
> 	- 在PHP中获取用户的IP地址，存储IP地址并禁止用户使用。调查代理或NAT背后的VPN和用户。
> 
> publicationDate: '2020-02-28 10:30:21'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '284c4642c3fa98a026ce5a9e6625bb16'

在PHP中，在基本水平上检测一个IP地址是非常容易的。

```php
echo '你知道，你的IP地址是' . $_SERVER['REMOTE_ADDR'] . '?';
```

> **警告：**获取IP地址作为`$_SERVER['REMOTE_ADDR']`字段的关键，只有当PHP是从浏览器中调用时才可能。在CLI模式下（例如，从终端与cron运行），IP地址是不可用的（这是有道理的，因为没有进行网络请求）。

可靠的IP地址发现
-----------------------------

经过多年的发展，我终于坚持了这种实现方式。

```php
function getIp(): string
{
    if (isset($_SERVER['http_cf_connecting_ip'])) { // 支持Cloudflare
        $ip = $_SERVER['http_cf_connecting_ip'];
    } elseif (isset($_SERVER['REMOTE_ADDR']) === true) {
        $ip = $_SERVER['REMOTE_ADDR'];
        if (preg_match('/^(?:127|10)\.0\.0\.[12]?\d{1,2}$/', $ip)) {
            if (isset($_SERVER['HTTP_X_REAL_IP'])) {
                $ip = $_SERVER['HTTP_X_REAL_IP'];
            } elseif (isset($_SERVER['http_x_forewarded_for'])) {
                $ip = $_SERVER['http_x_forewarded_for'];
            }
        }
    } else {
        $ip = '127.0.0.1';
    }
    if (in_array($ip, ['::1', '0.0.0.0', '本地主机'], true)) {
        $ip = '127.0.0.1';
    }
    $filter = filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_IPV4);
    if ($filter === false) {
        $ip = '127.0.0.1';
    }

    return $ip;
}
```

那就好得多了。

```php
echo '你知道，你的IP地址是' . getIp() . '?';
```

如果可以直接检测到IP，或只有IPv6，或处于CLI模式（如cron），则返回`127.0.0.1`（localhost）。

考虑到 "X-Forwarded-For "和 "X-Real-IP "头文件的实现直接在PHP中是非常危险的，因为数据很容易被修改，攻击者可以欺骗一个假的IP地址，例如，查看管理或激活网站的调试模式（Nette Tracy）。另一方面，我们必须接受一些代理请求（例如，当通过Cloudflare代理流量时，或在同一台机器上运行Apache和Ngnix时，当它们在本地被紧接着调用时）。

在用户直接访问服务器的情况下，只有一个正确的解决方案，那就是在Apache（通过`RemoteIP`扩展）和Nginx（通过`remote_ip`扩展）中确保`X-Forwarded-For`是由访问者的实际IP地址设置的，并且该IP地址不能用HTTP头来设置。

`$_SERVER['REMOTE_ADDR']`字段会自动获得正确的IP地址（即请求直接来自PHP的IP地址），我们不必处理它。

用户通过代理访问
----------------------------

经常发生的情况是，用户通过代理访问。然后，实际的IP地址被存储在变量`$_SERVER['HTTP_X_FORWARDED_FOR']`中。

例如，当服务器上的路由使用 "Ngnix -> Apache -> PHP "方法解决时，这种情况可能会发生，其中 "Ngnix "在 "Apache "之前充当一个反向代理。在这种情况下，PHP只看到内部网络中的IP地址（通常为`127.0.0.*`的形式）。

例如，**Cloudflare**服务可能会有这样的行为，应注意我们是在处理实际用户的IP地址还是代理的IP地址。对我来说，最好的方法是使用本文开头提到的`getIp()`函数。我们可以通过验证`$_SERVER['HTTP_CF_CONNECTING_IP']`密钥的存在来确保Cloudflare的检测，该密钥会在每个代理请求中自动传输。

VPN/代理检测
-------------------

对代理或VPN的使用没有可靠的检测，但在实际环境中，我们至少可以过滤掉一些流量。

有几种方法可以做到这一点。取一个IP地址的范围，比较请求来自的IP地址。

从一些VPN供应商那里，IP地址的清单是非正式的（见例如https://gist.github.com/JamoCA/eedaf4f7cce1cb0aeb5c1039af35f0b7），在Tor出口节点的情况下，是正式的（https://blog.torproject.org/changes-tor-exit-list-service，但Tor的桥梁不在那里）。

另一个选择是在某个地方进行在线请求，如果服务不正常，这既会延迟页面加载，也会向第三方 "泄露 "访问者的IP地址。从2023年起，我强烈建议不要采用这种方法，因为它开始变得更多的是保护和操纵用户数据。

这种在线查询可以是 "天真 "的，只需要看看谁拥有这个范围，或者它是否是一个代理/VPN（一些服务可能会返回这个，但默认情况下，它不是 "IP信息 "的一部分，例如从whois服务）。

(最）经常使用某种信誉评级，一些IP地址比其他IP地址更 "垃圾"。据统计，来自各种代理、VPN和Tor的垃圾比来自家庭IP地址的垃圾更多（也许 "被感染 "的家庭IP地址除外）。这样的声誉评估是由一些DNS屏蔽列表提供的，见一些随机列表，https://en.m.wikipedia.org/wiki/Comparison_of_DNS_blacklists 和 "上市目标 "栏，或者由Cloudflare等公司以 "僵尸管理 "等形式直接提供。

这在很大程度上取决于检测目标是什么。

IP地址存储
------------------

这取决于你有什么IP地址可用。

- IPv4的IP地址可以用4个字节存储，`ip2long'函数用于此。
- 然而，对于IPv6的IP地址，我们必须使用16个字节，而且没有转换功能。

如果你的数据库服务器不直接支持IP地址的数据类型，我建议将IP地址存储为`varchar(39)`，两个版本都适合作为一个字符串，并且是人类可读的。

> 当存储IP地址时，考虑是否有必要同时存储由`gethostbyaddr'函数检测的域名。在列举和搜索时，你无法找出这些名字，因为这需要很长的时间，而且它们会随着时间而改变。

阻止访问者的IP地址
-----------------------------

理想的解决方案是创建一个被封锁的IP地址列表，并在每次请求时将这个列表与当前的IP地址进行比较。如果地址匹配，请求将被立即停止。

```php
$blackList = [
    '第一个ip',
    'druha-ip',
];

if (\in_array(getIp(), $blackList, true) === true) {
    echo '不幸的是，你的ip地址被封锁了 :-(';
    die; // 退出请求
}
```

这个例子假设实现了上面例子中的`getIp()`函数。

一个更强大的解决方案是检查该索引在数组中的出现情况。

```php
$blackList = [
    '第一个ip' => true,
    'druha-ip' => true,
];

if (isset($blackList[getIp()]) === true) {
    echo '不幸的是，你的ip地址被封锁了 :-(';
    die; // 退出请求
}
```

服务器IP地址和服务器名称
---------------------------------

服务器的IP地址通常存储在`$_SERVER['SERVER_ADDR']`字段中，其名称可以通过`gethostbyaddr($_SERVER['SERVER_ADDR'])`结构获得。

但是，如果使用 "Ngnix -> Apache -> PHP "的概念，并且 "Ngnix "处于反向代理的角色，则不会显示服务器的真实IP地址。

在这种情况下，服务器的名称可以在`$_SERVER['SERVER_NAME']`字段中找到，或者通过使用`php_uname('n')`函数。[官方uname函数文档](https://www.php.net/manual/en/function.php-uname.php)。

然后我们可以使用这个技巧来找出服务器的公共IP地址：`gethostbyname(php_uname('n'))`。
