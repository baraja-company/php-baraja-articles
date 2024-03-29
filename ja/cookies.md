PHPにおけるCookie
=============

> id: '392dd88b-d2f5-4943-a993-01aaad7ccd32'
> slug:
> 	cs: cookies
> 	ja: phpniokerucookie
> 
> perex:
> 	- 'Cookies je krátká textová informace v paměti prohlížeče, kam můžeme v PHP zapisovat a označit si uživatele.'
> 	- クッキーとは、ブラウザのメモリにある短いテキスト情報のことで、ここにPHPでユーザーを書き込んでマークすることができるのです。
> 
> publicationDate: '2019-09-11 10:18:29'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '78f9aad0278acd51b98179e7d29fe3d8'

> **注意:** この記事は何年も前に書かれたもので、情報が古かったり間違っていたりすることがあります。このことを念頭に置いてお読みください。

クッキーは、ウェブサイト訪問者のブラウザに保存される小さなテキスト情報です。ページをリロードするたびに常に転送され、ユーザーがいつでも削除・変更・閲覧できるため、個人情報の保存にはあまり向いていません。

*警告：あなたのウェブサイトがユーザーまたは第三者のアドオン（例：Facebookのいいねボタン、Google Analyticsのトラフィックメーター、バナー広告）を追跡するためにクッキーを使用している場合、あなたはこれについてユーザーに通知する必要があります*。

> 「もうひとつ、あなたのサイトには、同意を得るまで広告や測定コードを入れてはいけません。そして、それは最悪だ。"
>
> -- <a href="https://phpfashion.com/jak-na-souhlas-s-cookie-ve-zkurvene-eu">David Grudl</a>.

クッキーから値を読み取る
--------------------------

すべてのクッキーはスーパーグローバル変数 `$_COOKIE` に格納され、各キーは配列として保存されます。

例えば、現在ログインしているユーザーの名前をクッキーの `user` キーの下に保存しておけば、簡単にそれを取得することができます。

```php
echo $_COOKIE['ユーザー'];
```

ご注意：クッキーは常に存在するとは限りません（例えば、お客様が新しいユーザーである場合など）。そのため、掲載前に必ずクッキーの存在を確認し、必要に応じて代替のエラーメッセージを提供する必要があります。

```php
if (isset($_COOKIE['ユーザー']) && $_COOKIE['ユーザー']) {
    echo 'ログインしているユーザーです。' . $_COOKIE['ユーザー'];
} else {
    echo '誰もサインインしていない';
}
```

利用可能なすべてのCookieを取得する
--------------------------------

すべてのクッキーはスーパーグローバル変数 `$_COOKIE` に格納されているので、簡単にリストアップすることができます。

```php
var_dump($_COOKIE);
```

あるいは、サイクルを繰り返して、すべてのキーと値を取得します。

```php
foreach($_COOKIE as $key => $value) {
    echo $key . ':' . $value;	// キーと値を書き出す
    echo '<br>。';				// 行を折り返す
}
```

Cookieに値を保存する
--------------------------

setcookie()`関数は、Cookieにデータを保存するために使用します。

最初のパラメータは `$_COOKIE` フィールドから読み込むためのクッキーキー、2番目のパラメータはデータそのものを文字列で指定します。

3番目のパラメータで、クッキーが利用できる有効期間を（オプションで）設定することができます。利用可能な時間は<a href="/date">timestamp</a>として与えられるので、この瞬間から1時間有効なクッキーを設定したい場合は、`time() + 3600`と書けばよいことになります。

```php
$data = '保存したいコンテンツがある。';

setcookie('テストクッキー', $data);
setcookie('テストクッキー', $data, time() + 3600);
```

より大きなデータを保存する
-------------------

クッキーは、より大きなデータを保存するのには適していません（ブラウザは通常4kB、最大20個のクッキーのみ保存可能で、サイズにはクッキー名、有効性設定なども含まれます）。

より大きなデータをサーバーに保存し、クッキーに識別子を入れるだけで、どのユーザーのものかを知ることができる方がよいのです。このメソッドは `$_SESSION` と呼ばれ、別の記事で説明されています。

必ずしもサーバーに常に同期してデータを保存する必要がない場合は、javascriptで利用できる**<a href="https://jecas.cz/localstorage">localstorage</a>**ストレージを利用することが可能です。容量は数MB程度で、データの有効期限はない。
