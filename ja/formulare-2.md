フォーム、PHPでのフォーム処理
================

> id: d1871a8d-bcfe-408d-ac12-6b827633f77e
> slug:
> 	cs: formulare-2
> 	ja: fomu-phpdenofomu-chu-li
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '91f43ee06328e88a6a1058ecbf028222'

HTMLフォームを作成し、それを送信して、今度はデータを処理したいとします。HTMLフォームの作成については、<a href="/formulare">別記事</a>があります。

データの受信 - さまざまな方法
----------------------------

フォームの送信方法は、HTMLで直接設定します。

2つのオプションがあります。

- GET** - アドレスバーでクエスチョンマークの後に表示されます。
 例：`php.baraja.cz/search.php?query=formulare`。
- **POST** - 非表示（見えない）、ほとんどのフォームは郵便で送られます。

その後、同じ方法でPHPで読み取る必要があります。

ユーザーからデータを取得し、スクリプトに転送する
------------------------------------------------------

基本はHTMLフォームで、その作り方は<a href="/formulare">別記事</a>をご覧ください。

手始めに、ユーザーの名前を入力する簡単なフォームを想定してみましょう。

```html
<form action="welcome.php" method="GET">
   Zadejte jméno: <input type="text" name="username">
   <input type="submit" value="odeslat">
</form>
```

テキストボックスが表示されるので、名前を入力し、送信をクリックします。ボタンがクリックされると、フィールドの内容がスクリプト `welcome.php` に送信されます。

では、実際に `welcome.php` ファイルで処理をしてみましょう。

```php
$username = $_GET['ユーザー名'];

echo '入力された名前は' . $username;
```

特別な変数 `$_GET` に注意してください。これは、フォームからのデータを格納する **スーパーグローバル変数** で、配列としてアクセスすることができる。

しかし、このソリューションの問題は、受信したデータが**安全ではない**ことであり、同様のフォームが簡単に攻撃される可能性があることです。例えば、潜在的な攻撃者が名前の代わりにjavascriptのコードをフィールドに入力すると、そのコードがページに書き込まれ実行されることがあります。

そのため、HTMLコードに出力する前に、必ずユーザーデータをサニタイズする必要があります。

```php
$username = $_GET['ユーザー名'] ?? '不明';

echo '入力された名前は' . htmlspecialchars($username);
```

さらなる加工
----------------

受信したデータに対して何でもできるし、普通の変数と同じように扱える。

例えば、2つのフィールドに値を追加します。

```php
echo $_GET['x'] + $_GET['y'];
```

また、ファイル、データベース、電子メール、...に保存します。

その際に便利なのが、以下の機能です。

- <a href="/file-put-contents">file_put_contents</a> - ファイルにデータを保存する関数です。
- <a href="/hashovani">MD5</a> - チェックサム計算、パスワード用など
- <a href="/cookies">Cookies</a>-データを**cookies**（ウェブブラウザ内の小さなファイル）に保存する。
