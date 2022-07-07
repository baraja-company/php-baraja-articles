文件_获取内容
=======

> id: '6c8889f1-95e7-4540-9c3a-0225c6383954'
> slug:
> 	cs: file-get-contents
> 	zh: wen-jian-huo-qu-nei-rong
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: f8b180a5f7208dab0d37761a5acdc9e3

**file_get_contents**函数用于读取一个文件并将其内容放入一个变量。这个函数类似于<a href="/include">include</a>函数，但与include不同的是，它可以检索互联网上的远程文件并通过变量传输其内容。

样品
------

这两个函数都可以用来从磁盘加载一个本地文件。

```php
$news = file_get_contents('新闻.html');

echo '最新新闻：<br>' . $news;
```

或从一个远程URL。

```php
$page = file_get_contents('https://www.google.com');

echo $page;
```

当检索一个URL时，任何地址都可以被下载，其内容可以作为一个字符串被检索到一个变量中。就HTML而言，这就是源代码。

页面呈现不正确
----------------------------

这是因为HTML代码是完全按照它在URL上的位置传递的。

如果图片的路径是`<img src="kocka.png">`，那么这个文件在我们的服务器上可能不存在，所以我们需要将路径改成例如：`<img src="https://server.cz/kocka.png">`。
