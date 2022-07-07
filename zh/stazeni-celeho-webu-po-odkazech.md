在PHP中通过链接下载整个网站
===============

> id: dd4abb8e-8f9b-4867-b98f-ff1c859d387a
> slug:
> 	cs: stazeni-celeho-webu-po-odkazech
> 	zh: zaiphp-zhong-tong-guo-lian-jie-xia-zai-zheng-ge-wang-zhan
> 
> publicationDate: '2019-11-06 17:41:30'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: e70451decff5c7bdf19bca8181c0dd5e

相对而言，我经常解决下载一个网站或域内所有页面的任务，因为我随后会用这些结果进行各种测量，或者用这些页面进行全文搜索。

一个可能的解决方案是使用现成的工具[Xenu](http://home.snafu.de/tilman/xenulink.html)，它很难安装在网络服务器上（它是一个Windows程序），或者使用[Wget](https://www.gnu.org/software/wget/)，它并不是到处都支持，而且会产生另一个不必要的依赖。

如果任务只是复制一个页面供以后查看，[HTTrack](https://www.httrack.com/)这个程序非常有用，我最喜欢它，只是当我们在索引参数化的URL时，在某些情况下会失去准确性。

所以我开始寻找一个可以直接在PHP中自动索引所有页面的工具，并进行高级配置。最终，这成为一个开源项目。

Baraja WebCrawler
-----------------

正是为了这些需求，我实现了自己的Composer包[WebCrawler](https://github.com/baraja-core/webcrawler)，它可以自己优雅地处理索引网页的过程，如果遇到新的情况，我会进一步改进它。

它是用Composer命令安装的。

```shell
composer require baraja-core/webcrawler
```

而且，它很容易使用。只要创建一个实例并调用`crawl()`方法。

```php
$crawler = new \Baraja\WebCrawler\Crawler;

$result = $crawler->crawl('https://example.com');
```

在`$result`变量中，完整的结果将作为`CrawledResult`实体的实例提供，我建议研究这个实体，因为它包含整个网站的许多有趣的信息。

抓取器设置
------------------

通常我们需要以某种方式限制网页的下载，因为否则我们可能会下载整个互联网。

这是通过使用 "Config "实体来实现的，该实体将配置作为一个数组（键值）传递给爬虫，然后由构造函数传递给爬虫。

比如说。

```php
$crawler = new \Baraja\WebCrawler\Crawler(
    new \Baraja\WebCrawler\Config([
        // key => value
    ])
);
```

设置选项。

| 关键 | 默认值 | 可能的值 |
|-------------------------|---------------|-----------------|
| `followExternalLinks` | `false` | `Bool`: 只停留在同一域内？它也能索引外部链接吗？
| "sleepBetweenRequests"| "1000"| "Int"：下载每个页面之间的等待时间，以毫秒计。
| `maxHttpRequests` | `1000000` | `Int`: 最多下载多少个URL？
| `maxCrawlTimeInSeconds` | `30` | `Int`: 最大下载时间是多少秒？
| `allowedUrls` | `['.+']` | `String[]`: 允许的URL格式阵列，作为正则表达式。
| "forbiddenUrls"| "['']"| "String[]": 禁止的URL格式阵列，作为正则表达式。

正则表达式必须与整个URL完全匹配，作为一个字符串。
