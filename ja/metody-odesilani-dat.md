データ送信方法（GET、POST）
=================

> id: '32f9083f-7cb1-469f-911a-765df059123d'
> slug:
> 	cs: metody-odesilani-dat
> 	ja: deta-song-xin-fang-fa-get-post
> 
> perex:
> 	- 'Metoda GET a POST, získávání dat z formuláře a URL. Komunikace přes API a zpracování dat.'
> 	- GET、POSTメソッド、フォームやURLからのデータ取得。 API通信とデータ処理。
> 
> publicationDate: '2019-11-26 11:38:32'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '81b5f92d7ee05563b6ece295ed5958d3'

通常の変数に加えて、PHPにはいわゆる**スーパーグローバル変数**があり、現在呼び出されているページと渡しているデータに関する情報を保持しています。

通常、ページ上にユーザーが入力するフォームがあり、そのデータをWebサーバーに転送し、PHPで処理することを考えます。

そのために最もよく使われる方法は3つあります。

- `GET` ~ データはパラメータとしてURLに渡される
- POST` ~ データはページリクエストと共に密かに渡される
- <a href="/ajax-post">Ajax POST</a> ～非同期javascript処理

GET メソッド - `$_GET`.
--------------------

GETメソッドで送信されるデータは、URLで（クエスチョンマークの後のパラメータとして）見ることができ、最大長はInternet Explorerで1024文字です（他のブラウザは*ほとんど*制限しませんが、大きなテキストはこの方法で渡すべきではありません）。この方法の利点は、ほとんどがシンプルであること（送信内容を確認できること）と、処理結果へのリンクを提供することができることです。データは変数に送られます。

受信ページのアドレスは、次のようになります。

`https://____________.com/script.php?promenna=obsah&promenna2=obsah`

そうすると、PHPでは、例えば、 `variable` パラメータの値を以下のように書くことができます。

```php
echo $_GET['プロムナード'];	// "コンテンツ "を表示する
```

> **強い警告:** HTMLページに直接データを書き込むこの方法は、例えば、ページに書き込まれて実行されるようなHTMLコードをURLで渡すことができるので、安全ではありません。
>
> ページに出力する前に必ずデータを処理しなければならないので、 `htmlspecialchars()` 関数が使用されます。
>
> 例： `echo htmlspecialchars($_GET['variable']);`.

POSTメソッド - `$_POST`.
----------------------

POSTメソッドで送信されるデータはURLで見えないため、送信データの最大長の問題を解決することができます。フォームフィールドの送信には常にPOSTメソッドを使用すべきです。これにより、例えばパスワードが見えないようにし、特定の入力結果を処理するページへのリンクを提供できないようにすることができます。

データは `$_POST` 変数に格納され、使い方は GET メソッドと同じである。

入稿データの有無の確認
--------------------------------

データを処理する前に、まずデータが実際に送信されたことを確認すべきです。そうでない場合、私たちはデータにアクセスすることになります。
 を存在しない変数に渡すと、エラーメッセージを投げることになります。

変数の存在を確認するには、`isset()`関数を使用します。

```php
if (isset($_GET['名称'])) {
    echo '君の名は。' . htmlspecialchars($_GET['名称']);
} else {
    echo '名前が入力されていません。';
}
```

データ入力フォーム
------------------------

フォームはPHPではなく、HTMLで作られています。プレーンなHTMLページでもよい。すべての "マジック "は、データを受け取るPHPスクリプトによって処理されます。

例えば、GETメソッドで送信された2つの番号を受け取るフォームを使用することができます。

```html
<form action="script.php" method="get">
    První číslo: <input type="text" name="x">
    Druhé číslo: <input type="text" name="y">

    <input type="submit" value="Sečíst čísla">
</form>
```

最初の行で、データがどこに、どのような方法で送信されるかを確認することができます。

次の2行は単純なフォーム要素で、**name=""**属性に注目してください。

次に、データを送信するボタン（必須）と、フォームを閉じるHTMLタグ（ブラウザが他に何を送信して、何を送信しないかを知るために必須）があります。

> 1つのページにいくつでもフォームを持つことができ、それらを入れ子にすることはできません。ネストが発生した場合は、常に最もネストされたフォームが送信され、残りは無視される。

サーバーでのフォーム処理
-------------------------------

これで完成したHTMLフォームを `script.php` に送信し、GETメソッドを使ってデータを受信します。ページ要求のアドレスは、次のようになります。

`https://________.com/script.php?x=5&y=3`

**script.php**

```php
$x = $_GET['x'];	// 5
$y = $_GET['y'];	// 3

echo $x + $y;		// 8を表示します。
```

正しくは、まず両方のフォームフィールドが埋め尽くされたことを確認する必要があります。これは `isset()` 関数で行います。

```php
if (isset($_GET['x']) && isset($_GET['y'])) {
    $x = $_GET['x'];	// 5
    $y = $_GET['y'];	// 3

    echo $x + $y;		// 8を表示します。
} else {
    echo 'フォームが正しく記入されていない。';
}
```

> **TIP:** `isset()` 構造体に複数のパラメータを渡して、それらがすべて存在することを確認することができます。
>
> したがって、`isset($_GET['x']) && isset($_GET['y'])` の代わりに、次のように指定すればよいことになります。
>
> `isset($_GET['x'], $_GET['y'])`.

POSTメソッドで受信したデータの処理
--------------------------------------

POSTメソッドでデータを受信した場合、処理するスクリプトのURLは常に次のようになります。

`https://________.com/script.php`

そして、決してそれ以外ではありません。ただ、ダメなんです。データはHTTPリクエストの中に隠されていて、私たちは見ることができないのです。

> セキュリティ上の理由から、ユーザー名やパスワードの送信にはhidden POSTメソッドが必要です。
>
> **Security:** サイトでパスワードを扱う場合、ログインと登録フォームはHTTPSでホストされるべきで、パスワードは適切にハッシュ化されなければなりません（例えば、BCryptを使用します）。

ajaxリクエストの処理
------------------------------

ajaxリクエストを処理する際、データを容易に取得できない場合があります。これは、ajax ライブラリが通常 `json payload` としてデータを送信するのに対し、スーパーグローバル変数 `$_POST` にはフォームデータのみが格納されるためです。

詳細は、<a href="/ajax-post">Processing ajax POST requests</a>の記事で説明しましたので、まだデータにアクセスすることができます。

生の入力を得る
-----------------------------

ユーザーが不適切な HTTP メソッドでリクエストを送信し、その上に独自の入力を追加することがあります。あるいは、例えば、バイナリファイルを送信したり、不正なHTTPヘッダを送信したりする。

このような場合は、PHPで次のように取得するネイティブ入力を使用するとよいでしょう。

```php
$input = file_get_contents('php://input');
```

REST APIライブラリを実装する中で、さまざまな種類のWebサーバーが入力HTTPヘッダを誤って判断したり、ユーザーがフォームデータを誤って送信したりするなどの特殊なケースにも遭遇しました。

この場合、私はこの関数を実装することで、ほぼすべてのケースを解決することができました（実装は `NetteHttpRequestFactory` に依存しますが、特定のプロジェクトでこの依存関係を他のものに置き換えることができます）。

```php
/**
 * HTTP ヘッダーから直接 POST データを取得するか、文字列からデータを解析しようとします。
 * レガシークライアントの中には、データを json として送信するものがあり、それは基本文字列形式であるため、フィールドを配列にキャストすることが必須となります。
 *
 * @return array<string|int, mixed>.
 */
private function getBodyParams(string $method): array
{
	if ($method === 'ゲット' || $method === 'デリート') {
		return [];
	}

	$request = (new RequestFactory())->fromGlobals();
	$return = array_merge((array) $request->getPost(), $request->getFiles());
	try {
		$post = array_keys($_POST)[0] ?? '';
		if (str_starts_with($post, '{') && str_ends_with($post, '}')) { // レガシークライアントのサポート
			$json = json_decode($post, true, 512, JSON_THROW_ON_ERROR);
			if (is_array($json) === false) {
				throw new LogicException('Jsonは有効な配列ではありません。');
			}
			unset($_POST[$post]);
			foreach ($json as $key => $value) {
				$return[$key] = $value;
			}
		}
	} catch (Throwable $e) {
		// 沈黙は金なり。
	}
	try {
		$input = (string) file_get_contents('php://input');
		if ($input !== '') {
			$phpInputArgs = (array) json_decode($input, true, 512, JSON_THROW_ON_ERROR);
			foreach ($phpInputArgs as $key => $value) {
				$return[$key] = $value;
			}
		}
	} catch (Throwable $e) {
		// 沈黙は金なり。
	}

	return $return;
}
```
