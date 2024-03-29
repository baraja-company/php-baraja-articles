Echo - ソースコードに出力する
==================

> id: d6a1a7bf-03bf-49ea-9947-d4d6753ca6e4
> slug:
> 	cs: echo
> 	ja: echo-sosukodoni-chu-lisuru
> 
> perex:
> 	- Echo - výpis řetězce do stránky a na standardní výstup. Možnosti escapování a HTTP hlaviček.
> 	- Echo - ページと標準出力に文字列をダンプします。エスケープとHTTPヘッダに関するオプションです。
> 
> publicationDate: '2020-02-16 18:40:31'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '7d611691825ec537f58f387696d13e28'

echo` 構造体は、変数や文字列をソースコードにダンプするために使用されます。

| サポート：｜すべてのバージョン
|----------------|------
| 簡単な説明：｜1つ以上の文字列を出力する。
| タイプ: <a href="/commands-and-functions">コマンド、コンストラクト</a> (関数ではありません)

商品説明
-----

```php
echo 'ハロー、ワールド';
```

hello world "と書いてある。

```php
$var = 'テキスト';
echo $var;
```

変数 `$var` の値、つまり "Text" を表示します。

**エコー**は関数ではありません（<a href="/commands-and-functions">コマンド</a>です）ので、括弧は使っても使わなくてもかまいません。したがって、`echo ('hello world');` と書くのも正しいです。

> **追記:** PHPは**Echo**をコマンド（構成要素）として扱うため、式として扱います。この場合、括弧は省略可能である。echo ('something');` という表記をした場合、**Echo**文は関数にならないので、そのように扱われません。この場合の括弧は、式の正確な値を囲むことを意味し、数学の仕組みと似ている。

引用符
--------

文字列は、引用符とアポストロフィで囲むことができる。

だから、これ。

```php
echo "ハイ";
```

これと同じです。

```php
echo 'ハイ';
```

ただし、各文字列は同じ種類の引用符で始まり、同じ種類の引用符で終わらなければならないので、注意が必要です。

例えば、HTMLのリンク（または任意のHTMLコード）を出力したい場合は、引用符の前にスラッシュを付けなければなりません。スラッシュは「まさにこの文字」という意味なので、言語では表現として理解されない。

```php
echo "<a href="index.php">リンクテキスト</a>を表示します。";
```

技術的なメモ: 引用符は、PHPでは<a href="/quotation-meaning">特別な意味</a>をもちます。

パラメータ
---------

- arg` 出力パラメータ。

戻り値
-----------------

値が返されない。

変数として使用することはできません。

備考
--------

注：これは**言語コンストラクト**（コンストラクト＝命令）であるため（関数ではない）、変数に読み込むことはできません。

例
-------

```php
echo "ハロー、ワールド";

echo "echo "は複数行のテキストを出力することができます。
ただし、HTMLタグの<br>は印刷されないので注意。そのためにnl2br()関数があるのです。";

$a = "php";				// 変数定義

echo "好き" . $a;	// と書いている。phpが好き
```

また、**Echo**は構文が短縮されており、phpの開始タグの後に等号のみを使用することが可能です。

```php
Ahoj <?=$jmeno;?>!
```

これは、ページに素早く情報を書き込む必要がある場合に便利です。例えば、今年度。

```php
Píše Jan Barášek © <?=date('Y');?>
```

> この短縮構文は、php の開始タグの短縮が有効な場合、つまり `short_open_tag` ディレクティブが `on` に設定されている場合にのみ機能します。

操作方法
-------

一般的な数学演算はすべて**echo**コマンドの中で実行することができます。

数学の詳細については、<a href="/mathematics">別記事</a>を参照してください。

```php
echo 5 + 3 * 2; // 11をプリントします。
```
