PHPでAjaxのPOSTリクエストを処理する
=======================

> id: c9c8fdb4-020d-4361-b425-4f4406a090ba
> slug:
> 	cs: ajax-post
> 	ja: phpdeajaxnopostrikuesutowo-chu-lisuru
> 
> publicationDate: '2019-11-01 09:56:02'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '89254da0b9b22b1926410b3ef7e511ec'

Vue.jsのajaxアプリケーションを開発する中で、何年か経ってようやくPHPでajaxを使う方法、POSTメソッドでデータを受け取る方法がわかりました。

スーパーグローバル変数 `$_POST` はフォームに対してのみ有効です。
-------------------------------------------------------------

PHP では、フォームから送信されたデータを保持するために <a href="/superglobal-variable">superglobal variable `$_POST`</a> が一般的に利用可能です。

使い方は**比較的**簡単です。

HTML側では、フォームを作成する必要があります。

```html
<form action="process.php" method="post">
    Jméno: <input type="text" name="username">
    <input type="submit" value="Odeslat">
</form>
```

そして、`process.php` ファイルでその値を配列の要素としてアクセスできるようにします。

```php
echo htmlspecialchars($_POST['ユーザー名'] ?? '');
```

> **警告:**
>
> この単純なアプローチでは、誰もが POST されたデータは自動的に `$_POST` 変数の配列インデックスとして定義されていると感じるかもしれません。しかし、そんなことはありません。

実際、POST メソッドを使ってフォームから送られたデータが `$_POST` 変数に書き込まれるのは、HTML フォームが送信されたときにブラウザが自動的に HTTP ヘッダー `'Content-Type': 'application/x-www-form-urlencoded'` を送信するためです。

ヘッダーが適切に設定されていないと、単純に値にアクセスすることができないため、トリック・ソリューションを使用する必要があるのです。

ajaxでデータを送信する
-------------------

ajaxを使ってデータを送ろうとする場合、PHP側で少しアプローチを変える必要があります。詳しくは、<a href="https://www.facebook.com/groups/frontendisti/permalink/2372671669611010/">Facebookでの議論</a>をご覧ください。

javascriptでは、例えば<a href="https://github.com/axios/axios">axios</a>というライブラリを使うと、ajaxを使ってデータを送信することができます。簡単に使うには、CDNサーバーからjavascriptをリンクして、そのまま使うだけです。

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

<script>
    axios.post('/api/form-process', {
        username: 'Jméno uživatele'
    })
    .then(response => {
        // Zpracování odpovědi z API
        alert(response.data.message); // Vyhodí hlášku se zprávou
    });
</script>
```

この単純なケースでは、URL `'/api/form-process'` が ajax で呼び出され、POST メソッドはオブジェクト `{ username: 'User name' }` を渡します。データを渡すための物流はすでにライブラリ自身が自動的に処理しているので、Jsonとしてシリアライズして送られます。フロントエンド開発者の用語では、これを **json payload** と呼びます。

PHP側では、フォームとして使うことを想定しています（なにしろ、POSTメソッドですから）。

```php
echo htmlspecialchars($_POST['ユーザー名'] ?? '');
```

ただし、この場合 `$_POST` フィールドは空となり、データは渡されません。変数 `$_POST` はフォームから取得したデータにのみ使用されます (捨てなかった HTTP ヘッダーを見ればわかります)。

そこで今回は、HTTPリクエストから直接データを取得する必要があり、そのために**トリック・ソリューション**が使用されています。すると、使い勝手がよくなります。

```php
$data = json_decode(file_get_contents('php://input'), true);

echo htmlspecialchars($data['ユーザー名'] ?? '');

header('Content-Type: application/json');
echo json_encode([
    'メッセージ' => 'サーバー・クリーパー',
]);
die;
```

例として、簡単なjavascriptでの回答も用意しています。重要なのは、HTTPヘッダ `'Content-Type: application/json'` を適切に投げ、すべてのデータが送信された後にスクリプトを終了させることです。

$_POST` の使用を強制する。
-------------------------

それでも投稿されたデータをそのままフォームとして扱いたい場合は、転送する方法があります。この場合、ajaxクエリの作成自体を修正し、HTTPヘッダを正しく渡す必要があります。

```js
axios.post(
    '/api/form-process',
    {
        username: 'Jméno uživatele'
    },
    {
        headers: {'Content-Type': 'application/x-www-form-urlencoded'}
    }
).then(response => {
    // Nějaké zpracování
});
```

しかし、この場合、データを修正して配列に変換する必要があるため、PHP側の処理は快適ではありません。

なんとかこのホラーを思いつきました。

```php
if (\count($_POST) === 1
    && preg_match('/^\{.*\}$/', $post = array_keys($_POST)[0])
    && ($json = json_decode($post)) instanceof \stdClass
) {
    foreach ($json as $key => $value) {
        $_POST[$key] = $value;
        unset($_POST[$post]);
    }
}

echo htmlspecialchars($_POST['ユーザー名'] ?? '');
```

しかし、最初の方法にこだわって `'php://input'` メソッドを使用してデータを取得する方がずっとよいでしょう。
