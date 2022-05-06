cURL in PHP - downloading data via URL
======================================

> id: '0bd1aed6-460d-4b63-9afe-f5087d1c6046'
> slug:
> 	cs: curl
> 	en: curl-in-php---downloading-data-via-url
> 
> perex: Knihovna cURL je robustní PHP knihovna pro pokročilou HTTP komunikaci.
> publicationDate: '2020-02-15 22:14:32'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '7c565449293cf1ce4950f309abaf04d0'

The PHP library `cURL` is a good way to download data from a foreign server.

Based on a query, it builds an HTTP request that it sends to the target server, and once downloaded, it contains an API for (relatively) easy data handling.

Unlike the native `file_get_contents` function (through which we can also make HTTP requests), it offers much better configuration options and downloads pages/files like a real browser.

> The `file_get_contents` function uses the `cURL` library internally, it just doesn't have as detailed configuration options.

Detecting cURL mode in a request
----------------------------

It is often useful to detect whether the current request was made via `cUrl` or classically in the browser.

There is no direct implementation for this in PHP, but we can write a simple function ourselves:

```php
function isCurl(): bool
{
    return str_contains($_SERVER['HTTP_USER_AGENT'] ?? '', 'curl');
}
```

If you have Linux and its Terminal, or are on a Mac, try this command:

```shell
curl https://php.baraja.cz/curl
```

The command makes an internal request to this site and returns the result.

If the application did not detect the cURL request, the HTML will be returned as if the request came from the browser. However, since request types are detected, there is nothing to prevent us from returning a cleaned up Markdown article.

The advantage then is much better cleaned data. We show the formatted HTML to the user in the browser, but only the basic content to the robot.

Detailed use of the API in PHP
--------------------------

If you are looking for detailed usage of cUrl, I recommend reading the <a href="https://www.php.net/manual/en/book.curl.php">official documentation</a>, which is always up to date.

For casual use, there is a <a href="https://docs.guzzlephp.org/en/stable/">**Guzzle**</a> library that handles
