PHPキーワード
========

> id: d58620d7-57a0-410a-ba73-31a1f9d984fb
> slug:
> 	cs: keywordy-jazyka-php
> 	ja: phpkiwado
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '38fbdb53b4455903f3300c3c8b4a8ad2'

ほとんどすべてのプログラミング言語は「キーワード」で構成されている。キーワードとは、特殊な意味を持つ言語の表現である。

キーワードの例としては、`string`、`if`、`echo`といった単語があります。

キーワード（時には `commands` も）<a href="/commands-and-functions"> は関数</a>ではないので、例えば `echo` は関数でもないことに注意することが重要です。

キーワードのリストは、PHPでは特別な意味を持ち、常にすべてに使用できるわけではないので、知っておくとよいでしょう。例えば、クラスの名前は、既存のキーワードの1つと同じであってはなりません。

パースエラーの例
-------------------

String` という名前のクラスを処理しようとすると、PHP はそれがクラス名なのかデータ型なのかわからないため、 `Parse error` が発生します。

これは間違っている。

```php
class String
{
   // ...
}
```

キーワード一覧
-------------------

PHP 7.1用のキーワードリストを更新しました。

and`, `or`, `xor`, `for`, `do`, `while`, `foreach`, `as`, `return`, `die`, `exit`, `if`, `then`, `else`, `elseif`, `new, `delete` などです。try`, `throw`, `catch`, `finally`, `class`, `function`, `string`, `array`, `object`, `resource`, `var`, `bool`, `boolean`, `int` などがあります。intger`, `float`, `double`, `real`, `string`, `array`, `global`, `const`, `static`, `public`, `private`, `protected`, `published` などがあります。拡張`、`スイッチ`、`true`、`false`、`null`、`void`、`this`、`self` 、`struct`、`char`、`signed`、`unsigned`、`short`、`long` など。
