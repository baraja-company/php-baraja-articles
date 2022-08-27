PHP での cURL - URL 経由でデータをダウンロードする
=================================

> id: '0bd1aed6-460d-4b63-9afe-f5087d1c6046'
> slug:
> 	cs: curl
> 	ja: php-deno-curl-url-jing-youdedetawodaunrodosuru
> 
> perex:
> 	- Knihovna cURL je robustní PHP knihovna pro pokročilou HTTP komunikaci.
> 	- cURL ライブラリは、高度な HTTP 通信を行うための堅牢な PHP ライブラリです。
> 
> publicationDate: '2020-02-15 22:14:32'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '7c565449293cf1ce4950f309abaf04d0'

PHPのライブラリ `cURL` は、海外のサーバからデータをダウンロードするのに適した方法である。

クエリに基づいてHTTPリクエストを構築し、ターゲットサーバーに送信する。ダウンロード後は、（比較的）簡単にデータを扱えるAPIを搭載している。

ネイティブの `file_get_contents` 関数 (これを通じて HTTP リクエストも可能) とは異なり、より優れた設定オプションを提供し、本物のブラウザのようにページやファイルをダウンロードします。

> `file_get_contents` 関数は内部で `cURL` ライブラリを使用しており、それほど詳細な設定オプションはありません。

リクエストに含まれるcURLモードの検出
----------------------------

現在のリクエストが `cUrl` を介して行われたのか、それとも古典的なブラウザで行われたのかを検出することは、しばしば有用である。

PHPにはこのための直接的な実装はありませんが、簡単な関数を自作することができます。

```php
function isCurl(): bool
{
    return str_contains($_SERVER['http_user_agent'] ?? '', 'カール');
}
```

LinuxとそのTerminal、またはMacをお使いの場合は、このコマンドを試してみてください。

```shell
curl https://php.baraja.cz/curl
```

このコマンドは、このサイトに対して内部リクエストを行い、その結果を返す。

アプリケーションがcURLリクエストを検出しなかった場合、ブラウザからリクエストがあったかのようなHTMLが返されます。しかし、リクエストタイプは検出されるので、クリーンアップされたMarkdownの記事を返すことを妨げるものは何もありません。

その結果、よりクリーンなデータが得られるというメリットがあります。ブラウザで見るユーザーには整形されたHTMLを見せ、ロボットには基本的な内容だけを見せるのです。

PHPでのAPIの詳細な利用方法
--------------------------

cUrlの詳しい使い方を知りたい方は、常に最新の情報を提供している<a href="https://www.php.net/manual/en/book.curl.php">公式ドキュメント</a>を読むことをお勧めします。

カジュアルな使い方としては、<a href="https://docs.guzzlephp.org/en/stable/">**Guzzle**</a> というライブラリがあります。
