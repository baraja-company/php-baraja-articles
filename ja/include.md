PHP Include - ページにファイルを挿入する
===========================

> id: '7a53145c-8552-425d-b864-283f73a7a7de'
> slug:
> 	cs: include
> 	ja: php-include-pejinifairuwo-cha-rusuru
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '6e18e67ec03b3a7592473230559fc364'

include` 構造体は、現在のページに追加のファイル/スクリプトを自動的に挿入します。

PHPに関する限り、自動的に実行され、常にその場所にあったかのように理解されます。

例

```php
include 'news.html';
```

実用化
-----------------

- 共通メニューとメニュー
- ニュース、アップデート、ニュース、同じ内容です。
- ヘッダーまたはフッター、...

ダイナミックなファイル読み込み
--------------------------

私たちはしばしば、例えば変数に基づいて動的にファイルをロードする必要があります。

例えば、こんな感じです。

```php
$clanek = '屍蝋人形';

include $clanek . '.html';
```

あるいは、ページに動的に記事を読み込むことも可能です。

```php
include '記事/' . $_GET['ページ'] . '.html';
```

page` パラメータを持つ URL が呼び出されると、例えば `https://example.com/?page=novinky` のように、記事が自動的に挿入される。
