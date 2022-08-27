ファイル_get_contents
=================

> id: '6c8889f1-95e7-4540-9c3a-0225c6383954'
> slug:
> 	cs: file-get-contents
> 	ja: fairu-get-contents
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: f8b180a5f7208dab0d37761a5acdc9e3

file_get_contents** 関数は、ファイルを読み込んでその内容を変数に格納するために使用されます。この機能は<a href="/include">include</a>機能に似ているが、includeとは異なり、インターネット上のリモートファイルを取得し、その内容を変数で転送することができる。

サンプル
------

いずれの関数も、ディスクからローカルファイルを読み込むために使用することができる。

```php
$news = file_get_contents('news.html');

echo '最新ニュース:<br>。' . $news;
```

またはリモートURLから。

```php
$page = file_get_contents('https://www.google.com');

echo $page;
```

URLを取得する場合、任意のアドレスをダウンロードし、その内容を文字列として変数に取得することができる。HTMLの場合、ソースコードにあたります。

ページが正しくレンダリングされない
----------------------------

これは、URL上に配置されたHTMLコードがそのまま渡されるからです。

画像へのパスが例えば `<img src="kocka.png">` の場合、このファイルはサーバーのコンテキストに存在しない可能性があるので、例えば `<img src="https://server.cz/kocka.png">` にパスを修正する必要があります。
