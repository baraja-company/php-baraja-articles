PHPの三項演算子（？
===========

> id: '1191be9b-ff9d-4725-b0b4-17b60de2b935'
> slug:
> 	cs: ternarni-operator
> 	ja: phpno-san-xiang-yan-suan-zi
> 
> perex:
> 	- Ternární operátor umožňuje zapsat jednoduchou podmínku do jednoho řádku jako výraz.
> 	- 三項演算子を使うと、簡単な条件を式として1行で書くことができます。
> 
> publicationDate: '2019-11-26 11:59:18'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: '20ff690401289869eae2a2584fae7568'

三項演算子を使うと、解析が不要、複雑、あるいは全く不適切な場所で、単純な条件を1行に短縮することができます。

TL;DR
------

三項演算子は `if` と `else` の条件の略記法なので、使う必要はありません。検証ロジックの繰り返しに有効です。常に括弧を含む`(条件 ? 正論理 : 負論理)`の書式を使用します。短い検証のために使用し、コードをより明確に短くします。

基本的な定義
------------------

というような形の条件をつけることがよくあります。

```php
$a = 5;
$b = 3;

if ($a > $b) {
     echo 'より大きくなった';
} else {
     echo '小さくなった';
}
```

つまり、単純な文章を1つだけ書こうとすると、4行のコードを使わなければならないのですが、それを1行に減らすことができるのです。

```php
$a = 5;
$b = 3;

echo 'です。' . ($a > $b ? '大きめ' : 'より小さい');
```

一般に三項演算子は3つの部分を使って記述します（だから "ternary "と呼ばれるのです）。

```php
(condition ? 'は' : 'から')
```

三項演算子は、表の偶数行を表すなど、実際には非常によく使われる。

```php
$pole = [3, 1, 4, 1, 5, 9, 2];

for ($i = 0; $pole[$i]; $i++) {
     echo '<tr class=""' . ($i % 2 ? 'から' : 'も') . '">';

          // これでなんとか表計算ソフトと連動...。
          // 例: echo '<td>' .$field[$i] . '</td>';

     echo '</tr';
}
```

三項演算子の使用例
------------------------------------

変数値の存在を確認し、必要であればすぐに使用する必要がある場合がかなりあります。存在しない場合は、デフォルト値を返したい。

古典的なやり方は、こんな感じです。

```php
$a = 5;
$b = 8;

$c = $a ? $a : $b;
```

変数 `$a` が存在する場合は `$a` を、そうでない場合は `$b` を値として使用します。

しかし、関数から値を取得することもある。

```php
$a = 5;
$b = 3;
$default = 42;

$c = my_function($a, $b) ? my_function($a, $b) : $default;
```

この呼び出し方は、システムリソースの面で非常に非効率的です。まず、関数を呼び出す必要があります。関数が存在する場合は、もう一度呼び出して値を取得し、その値を `$c` 変数に格納します。

これはヘルパー変数で処理したほうがよいでしょう。

```php
$a = 5;
$b = 3;
$helper = my_function($a, $b);
$default = 42;

$c = $helper ? $helper : $default;
```

不適切な使用
------------------

三項演算子は、複雑な条件や入れ子になった条件を書くときに混乱を招きがちなので、必ずしも使う価値があるとは言えません。

その一例をご覧ください。

```php
$valid = true;
$lang = 'フレンチ';

$x = $valid
     ? ($lang === 'フレンチ' ? 'ウイ' : 'は')
     : ($lang === 'フレンチ' ? 'ノン' : 'から');
```

変数 `$x` に値 `oui` が入ることは一目でわかるでしょうか？

少し練習すればできるようになるかもしれませんが、それは正しい答えではありません。:)

値の（非）存在を確認し、デフォルトを使用する
--------------------

三項演算子は、値の（非）存在確認や他のデフォルトの使用を日常的に行う際に最も威力を発揮する。

例えば、ある記事に対してメインカテゴリが存在するかどうかを確認し、存在しない場合は代替メッセージを出力したい。

```php
echo $mainCategory ?? 'カテゴリが存在しない';
```

演算子 `??` (二つの疑問符) は、変数 `$mainCategory` が存在し、かつ `null` でないかどうかをチェックします。これは `isset()` 関数と同じように動作します。

価値の空しさを検証する
-----------------------------

出力値が空ではない（つまり、`null`, `0`, `false` や `''*(empty string)*ではない）ことを確認するために、非常によく使われる便利な構文です。

これは次のように書くことができる。

```php
$a = 5;
$b = 3;
$default = 42;

$c = my_function($a, $b) ?: $default;
```

まず `function($a, $b)` が呼ばれ、その値がテストされ、空でなければ直ちに `$c` 変数に渡され、そうでなければ `$default` 変数が使われることになります。

演算子 `?:` は、条件式の `!=` 演算子と同じような働きをし（データ型に関係なく偽）、例えばエルビスヘアでスマイリーフェイスのような見た目で覚えることができる。

旧バージョンのPHPに対応
----------------------------

それ以前のバージョンでは、同じ動作をさせるためには `if (...)` 条件を使用しなければなりませんでした。三項演算子は、実際には、条件によって処理されるのと同じことを書くためのショートカットに過ぎないことを忘れないでください。

値が存在しないかどうかは `isset()` 関数で確認することができます。この関数は、変数が存在し、かつ空 (`null`) でないことを確認します。

コードの代わりに

```php
$a = 5;
$b = 3;

$c = $a ?? $b;
```

そして、古い方の選択肢を書き出す。

```php
$a = 5;
$b = 3;

$c = isset($a) && $a ?? $b;
```

> **Warning:** `isset()` と変数自体の順番が重要です。もし、順番を逆にして変数が存在しなかったら、存在しない変数へのアクセスエラーが発生することになります。
