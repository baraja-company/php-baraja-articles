PHPでリンクによるサイト全体のダウンロード
======================

> id: dd4abb8e-8f9b-4867-b98f-ff1c859d387a
> slug:
> 	cs: stazeni-celeho-webu-po-odkazech
> 	ja: phpderinkuniyorusaito-quan-tinodaunrodo
> 
> publicationDate: '2019-11-06 17:41:30'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: e70451decff5c7bdf19bca8181c0dd5e

一つのサイトやドメイン内の全ページをダウンロードし、その結果で様々な計測を行ったり、全文検索に利用したりすることが比較的多いからです。

一つの解決策として、既製のツール[Xenu](http://home.snafu.de/tilman/xenulink.html)を使用する方法がありますが、これはWebサーバーにインストールするのが非常に難しく（Windowsプログラムです）、または[Wget](https://www.gnu.org/software/wget/)はどこでもサポートされておらず、別の不必要な依存関係を生み出します。

後で見るためにページのコピーを取るだけなら、[HTTrack](https://www.httrack.com/)というプログラムが非常に便利で、私はこれが一番気に入っています。ただ、パラメータ付きのURLをインデントする場合、場合によっては精度が落ちることがあります。

そこで、高度な設定により、PHPで直接すべてのページを自動的にインデックスすることができるツールを探し始めたのです。やがて、これはオープンソースのプロジェクトとなった。

バラハ・ウェブクローラー
-----------------

まさにこのようなニーズのために、私は独自のComposerパッケージ[WebCrawler](https://github.com/baraja-core/webcrawler)を実装しました。これは、それ自体でページのインデックス付け処理をエレガントに処理することができ、新しいケースに出会えば、さらにそれを改良しています。

Composerコマンドでインストールされます。

```shell
composer require baraja-core/webcrawler
```

そして、使い勝手の良さ。インスタンスを生成して `crawl()` メソッドを呼び出すだけでよい。

```php
$crawler = new \Baraja\WebCrawler\Crawler;

$result = $crawler->crawl('https://example.com');
```

変数 `$result` には、完全な結果が `CrawledResult` エンティティのインスタンスとして格納されます。これは、サイト全体に関する興味深い情報をたくさん含んでいるので、研究することをお勧めします。

クローラー設定
------------------

多くの場合、ページのダウンロードを何らかの方法で制限する必要があります。そうしないと、インターネット全体をダウンロードしてしまう可能性があるからです。

これは `Config` エンティティを用いることで実現される。Config エンティティは設定を配列 (key-value) として渡し、コンストラクタによってクローラに渡す。

例えば、こんな感じです。

```php
$crawler = new \Baraja\WebCrawler\Crawler(
    new \Baraja\WebCrawler\Config([
        // キー => 値
    ])
);
```

設定オプション。

| キー｜初期値｜設定可能な値｜｜｜。
|-------------------------|---------------|-----------------|
| `followExternalLinks` | `false` | `Bool`: 同じドメイン内のみに留まるか？外部リンクもインデックスできるのか？
| `sleepBetweenRequests` | `1000` | `Int`: 各ページをダウンロードする間の待ち時間（ミリ秒単位）。
| `maxHttpRequests` | `1000000` | `Int`: ダウンロードしたURLの最大数?
| `maxCrawlTimeInSeconds` | `30` | `Int`: ダウンロードの最大時間を秒単位で指定します。
| `allowedUrls` | `['.+']` | `String[]`: 正規表現として許可されるURLフォーマットの配列。
| `forbiddenUrls` | `[']` | `String[]`: 禁止されたURLフォーマットの正規表現による配列。

正規表現は、URL全体を文字列として正確にマッチさせる必要があります。
