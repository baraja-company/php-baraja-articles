PHP中的cURL - 通过URL下载数据
=====================

> id: '0bd1aed6-460d-4b63-9afe-f5087d1c6046'
> slug:
> 	cs: curl
> 	zh: php-zhong-decurl-tong-guourl-xia-zai-shu-ju
> 
> perex:
> 	- Knihovna cURL je robustní PHP knihovna pro pokročilou HTTP komunikaci.
> 	- cURL库是一个强大的PHP库，用于高级HTTP通信。
> 
> publicationDate: '2020-02-15 22:14:32'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '7c565449293cf1ce4950f309abaf04d0'

PHP库`cURL`是一个从国外服务器下载数据的好方法。

基于查询，它建立了一个HTTP请求，并将其发送到目标服务器，一旦下载，就包含一个API，用于（相对）容易的数据处理。

与原生的`file_get_contents'函数不同（我们也可以通过该函数进行HTTP请求），它提供了更好的配置选项，并像真正的浏览器一样下载页面/文件。

> `file_get_contents`函数内部使用`cURL`库，只是没有那么详细的配置选项。

检测请求中的cURL模式
----------------------------

检测当前的请求是通过`cUrl'还是在浏览器中进行的，通常很有用。

在PHP中没有直接的实现，但我们可以自己写一个简单的函数。

```php
function isCurl(): bool
{
    return str_contains($_SERVER['http_user_agent'] ?? '', '卷曲');
}
```

如果你有Linux和它的终端，或在Mac上，试试这个命令。

```shell
curl https://php.baraja.cz/curl
```

该命令向该网站发出内部请求并返回结果。

如果应用程序没有检测到cURL请求，HTML将被返回，就像请求来自浏览器一样。然而，既然检测到了请求类型，就没有什么可以阻止我们返回一个经过清理的Markdown文章。

这样做的好处是可以更好地清理数据。我们在浏览器中向用户展示格式化的HTML，但只向机器人展示基本内容。

在PHP中对API的详细使用
--------------------------

如果您想了解cUrl的详细用法，我推荐您阅读<a href="https://www.php.net/manual/en/book.curl.php">官方文档</a>，它总是最新的。

对于休闲使用，有一个<a href="https://docs.guzzlephp.org/en/stable/">***Guzzle***</a>库可以处理
