アポストロフィーとクォーテーションマーク
====================

> id: '526ad995-3412-446e-bb56-9627dff8e29e'
> slug:
> 	cs: apostrofy-a-uvozovky
> 	ja: aposutorofitoku-oteshonmaku
> 
> perex:
> 	- Použití uvozovek a apostrofů pro ohraničení řetězců v PHP. Escapování řetězců a řešení kontextů.
> 	- PHP で文字列を区切るために引用符とアポストロフィーを使用する。 文字列のエスケープとコンテキストの解決。
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c5b0bf8b74238134be5348f886591e2a

文字列の区切りには、引用符とアポストロフィのどちらかを使用することができます。個人的には、特殊な改行文字やタブでない限り、**アポストロフィー**だけが好ましいと思います。

その理由はいくつかありますが、建設的に考えて行きましょう。

> 引用符を使用しない主な理由は、セキュリティとデータ型の不適切な取り扱いです。

文字列の中でHTMLタグを使用する
--------------------------------

HTMLコードを文字列で返す必要がある場合、文字列を引用符で囲むと、かなり厄介なエスケープをしなければならなくなります。

```php
return "<a href=\"{$link}\">{$text}</a>";
```

または、同じことをより読みやすい方法で、引用符をエスケープせずに保持します。

```php
return '<a href="' . $link . '">' . $text . '</a>';
```

変数から文字列の視覚的距離が長い
---------------------------------------------

文字列の中にある変数を直接重ねると、何が変数で何が文字列なのか、視覚的にすぐに分からなくなるのが困りものです。

```php
$url = "{$baseImageUrl}/{$dirName}/{$basename}.{$ext}";
```

この表記は、機能的には何の悪影響もないが、文字列から個々の変数や固定文字を積み重ねすぎて可読性が低下することだけは確かである。

もう一度、読みやすいように工夫してみよう。

```php
$url = $baseImageUrl . '/' . $dirName . '/' . $basename . '.' . $ext;
```

私が考える最大のメリットは、名前の中に不要な文字がなく、変数周りがすっきりしていることです。

さらに、ラップが簡単になり、文字列の折り返し文字がなくなります ;)

```php
$url = $baseImageUrl
       . '/' . $dirName
       . '/' . $basename . '.' . $ext;
```

引用符の中で関数を呼び出すことはできません。
---------------------------------------

そのため、"something "は文字列の外側に付加されるが（特に関数）、変数は内側に書き戻される。そして時には、プログラマーが文字列の後に変数を追加することさえある。要するに、混乱に混乱を重ねるということです。

なぜ、画一的なことができないのでしょうか。

他のデータ型から文字列への不適切なマッピング
---------------------------------------------------

次のような関数呼び出しを考えてみましょう。

```php
echo getFullName("{$user->name}");

function getFullName(string $name): string
{
	// ...実装 ...
}
```

変数を引用符で囲んで挿入することも可能で、その場合は文字列として書き直される。ただし、変数が文字列自体の中にあって、stringとは異なるデータ型である場合、元のデータ型に関する情報が壊れる可能性があります。例えば、 `$user->name` の構文が `false` や `null` を返した場合、それがエラーであることは分かりません。

アプリケーションは、エラー状態を無視するのではなく、常にエラー状態を認識し、適切に失敗する必要があります。エラー報告が適切に行われることは、将来的なデバッグの改善につながります。

時には、関数が特定のデータ型を必要とするために、オーバーライドに遭遇することがあります。

```php
trim("{$returnText}");
```

それならいっそのこと、書いてしまおうという気になる。

```php
trim ((string) $returnText);
```

というのは、それほど「魔法のような」ものではなく、変数に何が起こったかは、あまり熟練していないユーザーにも明らかです。

データベースから値 `null` を削除する
----------------------------------

例えば、データベースからホテルの名前を取り出すとする。

```php
return "{$row->hotel->name}";
```

しかし、その名前が存在せず、`null`であった場合はどうでしょうか？引用符を使うことで、空の文字列に書き換えられてしまうため、発見されにくい可能性のあるエラーを作り出しています。しかし、私たちは本当の `null` を返したいのです。レコードが存在しないか、存在するが空であるかの違いです。

ですから、何も指定しないのが正しい方法でしょう。

```php
return $row->hotel->name;
```

その関数は、特定のデータ型を返す必要があり、それについて厳密ですか？仕様を満たせない場合は、例外を投げる必要があります。

```php
function getHotelName(int $hotelId): string
{
   // 実装

   if ($row->hotel->name === null) {
      throw new \Exception('ホテル名が存在しない。');
   }

   return $row->hotel->name;
}
```

文字とデータ型のエンコーディングの不整合
--------------------------------------------

というような構文に出会うことも少なくない。

```php
echo "{$returnCode}" . self::CRLF;
```

と書いたほうがいい。

```php
echo $returnCode . "\n";
```

この場合、変数の周りに引用符を使うのは不要で、余計な文字が増えるだけなので、読みにくくなります。同時に、 `CRLF` 型の行の折り返しは現代的ではなく、 `UTF-8` エンコーディングにネイティブな `CRLn` のみを使用するのがよいでしょう。

きちんと、コードのデータ型を検証することはできました。例えば、それが数字であること、場合によっては置換を返すこと。これは、<a href="/ternary-operator">三項演算子</a>で行うことができます。

```php
echo (is_int($returnCode) ? $returnCode : '?') . "\n";
```

> 注意: `is_int()` 関数を使用することは、アプリケーションの設計が悪いことを意味します。

出力文字列をアポストロフィで折り返す
---------------------------------------

変数からの出力文字列を引用符で囲んだり、アポストロフィで囲んだりすることが、例外文の中でしばしば必要になります。しかし、これでは不必要に両者の文字列区切りに「ペイオフ」が発生し、文字列の始まりと終わりが同じ文字である場合、内部にアポストロフィが現れると、複数の文字列の場合、どこで始まり、どこで終わったかを曖昧にせずに迅速に判断することができない。

```php
throw new \Exception(__METHOD__ . ": Unsupported data type '{$fileType}'");
```

私自身は、文字列の始まりと終わりがエレガントに見えるように、別の文字型（角括弧がよく効きます）で囲むことでこれを解決しています。

```php
throw new \Exception(__METHOD__ . ': サポートされていないデータ型 [.' . $fileType . ']');
```

あるいは

```php
throw new \Exception("Status '{$status}' is invalid");
```

との交換が可能です。

```php
throw new \Exception('ステータス' . $status . 'は無効です。');
```

行単位でのパース
--------------------

引用符は、改行で解析する場合などに便利です。

```php
foreach(explode("\n", $text) as $line) {
	// 実装
}
```

このために特別に作られたものです。

しかし、より複雑な文書（プログラミング言語のソースコードやHTMLページなど）を処理する必要がある場合は、<a href="/regex">正規表現</a>と一緒に**Tokenizer**を使用することをお勧めします。

2つの囲み方を組み合わせないでください
-----------------------------------

もし、何らかの理由で引用をするのであれば、せめて全体的に統一してください。

これは抑止力のあるケースです。

```php
throw new \Exception("URLの画像が存在しない。レスポンスサイズ。" . strlen($result) . ')');
```

ここで、文字列の先頭は引用符で、末尾はアポストロフィで区切られる。

この不具合は、特に自動組版ツール（**Code Sniffer**など）を使用した場合に発生します。
