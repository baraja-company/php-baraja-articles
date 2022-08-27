PHPでの定数の安全でない使用
===============

> id: b4d0f45f-c059-4a47-9b63-8980d0d465ed
> slug:
> 	cs: nebezpecne-konstanty
> 	ja: phpdeno-ding-shuno-an-quandenai-shi-yong
> 
> publicationDate: '2021-06-22 10:49:46'
> mainCategoryId: '561b0614-e152-4a62-a634-1f2d605d39d9'
> sourceContentHash: '58d4ea2e4bf106402386506b023ba4d2'

PHP で定数を使用する際には、2 つの厄介なことに注意しなければなりません。

動的定数と静的定数
------------------------------

PHP で定数を定義するには、クラス内で直接静的に定義する方法 (これが一番よい方法) と、次のようにする方法があります。

```php
class Region
{
	public const PREFIX = 420;
}
```

そして、その用途は極めて明確です。クラスのコンパイル時に定数の値が決まっており、クラス名と定数そのものを呼び出すことでアクセスできる。多くの場合、`Region::PREFIX`と記述することで実現します。

もう一つの方法（もっと悪い方法）は、実行時に定数を動的に定義することです（多くの場合、設定スクリプトのどこかにあります）。

```php
define('BASE_DIR', __DIR__ . '/../');
```

define` 関数で定数を定義することの大きな欠点は、定数を定義したスクリプトが呼び出されていない可能性があり、定数を読み込もうとしたときに定数が存在しないことです。

クラス内の静的定数の定義の中で動的定数を使用すると、致命的なリフレクションエラーになることさえあります。

```php
class InvoiceGenerator
{
	// これは完全に間違っている！
	public const DATA_DIR = BASE_DIR . '/データ/インボイス';
}
```

> **説明:**。
>
> 静的定数の中で動的定数を使用すると、コンパイル時に動的定数の値を読めないという大きな欠点があります。そのため、このスクリプトはリクエストごとに再度処理されなければならず（つまり、スピード最適化のためにOPCacheに保存することはできない）、定数が全く存在しなかった場合は致命的なコンパイル時エラーが投げられ、アプリケーションは全く実行できなくなる。

PhpStanを使用すると、この問題について自動的に警告を出すことができます。

```txt
Reflection error: Could not locate constant "BASE_DIR" while
evaluating expression in InvoiceGenerator at line 6
```

> **学習：***
>
> すべての定数の値は常に一定であるべきです。


staticを使用した場合の定数の継承
-------------------------------------

場合によっては、定数の値をオーバーライドするために、継承を使用することが理にかなっています。しかし、その場合、祖先は子孫から値を読み取ることができない（あるいはしてはいけない）。

例えば、国や地域を定義する場合です。

```php
abstract class Region
{
	public function getPrefix(): int
	{
		// 致命的なミス!
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

上記のコードは必ずしもエラーを投げないが、不適切な継承の使用によってエラーを投げられるというパラドックスである。

CzechRepublic` の子孫に対して `getPrefix()` メソッドを呼び出すと、定数の値が正しく読み込まれるので、すべてが正しく行われます。しかし、子孫が定数値を設定しなかった場合、致命的な非存在定数エラーが投げられることになります。全体の中で最悪なのは、メソッドの実装の中で作られる **隠れ依存関係** であり、クラスを継承する開発者はこの依存関係を知らないかもしれないことです。

この場合の最良の解決策は、祖先で直接定数を定義してデフォルト値を設定するか（ロジックが常に通るように）、少なくともゲッターで例外を発生させることです。

```php
abstract class Region
{
	public const REGION = null;

	public function getPrefix(): int
	{
		if (static::REGION === null) {
			throw new \LogicException('地域は定義されていません。');
		}
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

PhpStanはこのエラーに対して次のように応答します。

```txt
Access to undefined constant static(Region):REGION.
```
