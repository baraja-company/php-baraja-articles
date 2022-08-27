構成は以下の通りです。
===========

> id: '881cc6ef-b1dc-4a71-ab22-d1943ce8095b'
> slug:
> 	cs: include-function
> 	ja: gou-chengha-yi-xiano-tongridesu
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '53294e511809a77c08883100fd7416c6'

| サポート｜PHP 4、PHP 5、PHP 7
|---------------|---------
| 短い説明｜別のテキストファイルまたはスクリプトをスクリプトに追加します。
| 要件｜挿入する他のテキストファイルやスクリプト。
| 注意｜外部ファイルを読み込むことができません。

商品説明
--------------------------

別のテキストファイルやスクリプトをページに挿入します。プレーン/テキストファイルをサポートします。

埋め込まれたファイルは、あたかもページ内に直接存在するかのように動作します。

埋め込まれたスクリプトは自動的に実行されます。

埋め込みスクリプトは、変数の値を転送します。

> 外部ストレージから読み込むことができません。フォーマットされていないプレーンなテキストとPHPファイルのみ読み込み可能です。

許可されたパス形式。

- `script.php` - 同じディレクトリにあるファイル、実行されます。
- `script.html` - 同じディレクトリにあるファイルですが、実行されません。
- `./file.php` - 同じディレクトリにあるファイルが実行されます。
- `../page.html`
- `foldersfile.php` - write in Windows
- アドレス/DalsiAddress/file.php` - Unixシステムで書き込みます。

スラッシュのタイプ `****` と `**/**` は相互変換が可能なので、そのような処理をする必要はありません。

不正なパスエントリ：`https://domena.pripona/slozka/soubor.php`。

類似機能
--------------------------

- <a href="/file-get-contents">file_get_contents()</a>の場合。

例
--------------------------

```php
include 'file.php';
```

これは `file.php` スクリプトをページに挿入して実行します。

`vars.php`

```php
$color = '碧い';
$fruit = 'アップル';
```

`test.php`

```php
$color = '';
$fruit = '';

echo 'A' . $color . '' . $fruit; // A

include 'vars.php';

echo 'A' . $color . '' . $fruit; // 青リンゴ
```

> 警告：以下の表記は不可能です。変数の内容を定義して転送してください!

```php
include 'file.php?parameter=neco';
```

戻り値
--------------------------

なし、ファイルを置くだけです。

**注：**ファイルの内容を比較することができますが、これはセキュリティ上のリスクがあります。括弧の順番が重要!例

```php
if ((include 'file.php') == 'よっしゃー') {
    echo '値は "OK "です。';
}
```

> これは潜在的なセキュリティリスクです
>
**file_get_contents()**, **readfile()** or **fopen()** で解決可能です。Fopen() は、.txt と .html ファイルにのみ使用する必要があります。
>
> より安全な読み出しは、カスタム関数を定義することで解決できます。

例

```php
$string = get_include_contents('somefile.php');

function get_include_contents(string $filename): ?string
{
    if (is_file($filename)) {
        ob_start();
        include (string) $filename;
        return ob_get_clean();
    }

    return null;
}
```

他の開発者からのメモやヒント
--------------------------

**Jakub Vrána**からメールが届きました。

```php
include '記事/' . $_GET['記事'] . '.html';
```

これは非常に危険なことです。

攻撃者は、記事名として `../` などを使って別のディレクトリへのリンクを渡すことができ、最後にヌルバイトを渡すことで末尾を消すことができる場合もあります。

少なくとも `basename()` 関数は使うべきですが、ホワイトリストの値だけを許可するのがベターです。
