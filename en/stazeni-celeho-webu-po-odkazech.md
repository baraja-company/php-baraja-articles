Downloading the whole site by links in PHP
==========================================

> id: dd4abb8e-8f9b-4867-b98f-ff1c859d387a
> slug:
> 	cs: stazeni-celeho-webu-po-odkazech
> 	en: downloading-the-whole-site-by-links-in-php
> 
> publicationDate: '2019-11-06 17:41:30'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: e70451decff5c7bdf19bca8181c0dd5e

Relatively often I solve the task of downloading all pages within one site or domain, because I then perform various measurements with the results, or I use the pages for full-text search.

One possible solution is to use the ready-made tool [Xenu](http://home.snafu.de/tilman/xenulink.html), which is very difficult to install on a web server (it is a Windows program), or [Wget](https://www.gnu.org/software/wget/), which is not supported everywhere and creates another unnecessary dependency.

If the task is just to make a copy of the page for later viewing, the program [HTTrack](https://www.httrack.com/) is very useful, which I like the most, only when we are inding parameterized URLs we can lose accuracy in some cases.

So I started looking for a tool that can index all pages automatically directly in PHP with advanced configuration. Eventually, this became an opensource project.

Baraja WebCrawler
-----------------

For exactly these needs I implemented my own Composer package [WebCrawler](https://github.com/baraja-core/webcrawler), which can handle the process of indexing pages elegantly on its own, and if I come across a new case, I further improve it.

It is installed with the Composer command:

``shell
composer require baraja-core/webcrawler
```

And it's easy to use. Just create an instance and call the `crawl()` method:

```php
$crawler = new \Baraja\WebCrawler\Crawler;

$result = $crawler->crawl('https://example.com');
```

In the `$result` variable, the complete result will be available as an instance of the `CrawledResult` entity, which I recommend to study because it contains a lot of interesting information about the whole site.

Crawler settings
------------------

Often we need to limit the downloading of pages somehow, because otherwise we would probably download the whole internet.

This is done by using the `Config` entity, which is passed the configuration as an array (key-value) and then passed to the Crawler by the constructor.

For example:

```php
$crawler = new \Baraja\WebCrawler\Crawler(
    new \Baraja\WebCrawler\Config([
        // key => value
    ])
);
```

Setting options:

| Key | Default Value | Possible Values |
|-------------------------|---------------|-----------------|
| `followExternalLinks` | `false` | `Bool`: Stay only within the same domain? Can it index external links as well? |
| `sleepBetweenRequests` | `1000` | `Int`: Wait time between downloading each page in milliseconds. |
| `maxHttpRequests` | `1000000` | `Int`: How many maximum downloaded URLs? |
| `maxCrawlTimeInSeconds` | `30` | `Int`: How long maximum download in seconds?
| `allowedUrls` | `['.+']` | `String[]`: Array of allowed URL formats as regular expressions. |
| `forbiddenUrls` | `['']` | `String[]`: Array of forbidden URL formats as regular expressions. |

The regular expression must match the entire URL exactly as a string.
