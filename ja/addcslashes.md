アドクスクラッシュ
=========

> id: '48278937-b1c5-479b-bac2-9b1ec552df4c'
> slug:
> 	cs: addcslashes
> 	ja: adokusukurasshu
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1f53fe14dd5fbf79958c645d15c4d204'

PHP4、PHP5対応

`addcslashes` - C スタイルのスラッシュ文字列

商品説明
--------------------------

```php
string addcslashes (string $str, string $charlist)
```

charlist パラメータで指定された文字の前にバックスラッシュを付加した文字列を 返す。

パラメータ
--------------------------

**str** テキスト文字列

**charlist**

の文字が削除されます。charlistに `n`, ``rr` などの文字が含まれている場合は、C-styleに変換される。その他の英数字以外の ASCI 文字で、長さが 32 未満かつ 126 を超えるものは変更される。

charlistの引数で文字列を定義する場合、どのような文字を範囲の最初と最後に置くかを確認してください。

```php
echo addcslashes('foo[ ]です。', 'A..z');

// Values: \fo[ ]。
// すべての小文字と大文字を削除します
```

戻り値
--------------------------

変更された文字列を返す。

例
--------------------------

```php
$escaped = addcslashes($not_escaped, "\0..\37!@\177..\377");
```

charlist ``Camera0..\37!@177..\377``, ASCIIコード0〜31の文字を全て削除します。
