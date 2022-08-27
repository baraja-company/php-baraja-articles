電話番号のバリデーションとフォーマット
===================

> id: bbfe35c3-3a8c-4fe5-a9bd-5bff0fc234e0
> slug:
> 	cs: telefonni-cisla
> 	ja: dian-hua-fan-haonobarideshontofomatto
> 
> publicationDate: '2021-06-18 09:20:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '224b3857a6bb898d9d322499cd1d3757'

PHPで電話番号を検証し、フォーマットする簡単な方法はありません。そこで、依存関係がなく、かつこの役割を果たすことができる簡単なライブラリを書きました。

目的は、電話番号の形式をチェックしたり、基本的な正規の形式（常に有効）に変換したりすることである。

インストール
---------

単純に作曲家別。

```txt
$ composer require baraja-core/phone-number
```

または、[Download on GitHub](https://github.com/baraja-core/phone-number)パッケージをダウンロードしてください。

図書館の利用方法
----------

このツールの原理は、ユーザーからの電話番号、またはあなたがコントロールできないソースからの電話番号をフォーマットし、検証することに基づいています。

最も一般的な使用方法は、電話番号の書式を修正することです。

```php
$original = '+420 777123456';
$formatted = \Baraja\PhoneNumber\PhoneNumberFormatter::fix($original);

echo $original . '<br>。';
echo $formatted;
```

この関数は数値の書式を修正し、文字列 `+420 777 123 456` を返します。

ユーザがプリフィックスを指定しなかった場合、`+420`のプリフィックスが指定されたとみなされる。2番目のパラメータで、デフォルトのプリファレンスを変更することができます。

```php
// リターン：+421 777 123 456
\Baraja\PhoneNumber\PhoneNumberFormatter::fix('+420 777123456', 421);
```

電話コードは、ユーザーが入力せず、自動検出に失敗した場合のみ、上書きされます。

入出力フォーマット
----------------------------

入力文字列は（ほとんど）どのようにでも見える。内蔵のアルゴリズムは、有効でない文字を自動的に削除することができます（例えば、電話番号の横にメモを書くユーザーがいますが、これは自動的に削除されます）。 そのため、入力のフォーマットを気にする必要は全くありませんが、出力は常に一貫したものになります。

出力は常に同じに見えます（正規の形式に正規化されています）。

一般的なフォーマットは

```txt
   +420 777 123 456
     |  \_________/
  Prefix     |
      National number
```

有効でない入力（または自動的に修正できない入力）を送信した場合、例外がスローされます。

エラートラッピング
----------------

数値が安全に基本形に正規化できない場合や、存在しない場合は、 `InvalidArgumentException` 例外を投げる。

例外をブール値に変換したい場合は、組み込みのアセットバリデーターを使用します。

```php
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('123'); // false
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('777123456'); // 真
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777123456'); // 真
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777 123 456'); // 真
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 77 712 34 56'); // 真
```
