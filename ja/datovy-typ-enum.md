PHP のデータ型 Enum オブジェクト
=====================

> id: '5b96765c-16c2-4f76-8e31-6de6e9f59365'
> slug:
> 	cs: datovy-typ-enum
> 	ja: php-nodeta-xing-enum-obujekuto
> 
> publicationDate: '2022-05-29 15:30:00'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: '43735e926db87d2cc6c538ddb67e0a69'

PHP 8.1 以降、Enum データ型を使用してリストの正確な列挙値を定義することができます。これは、ある変数の値が特定のいくつかの値しかとれないことがわかっている場合に有効です。

例えば、通知の種類はこのように保存しています。

```php
enum OrderNotificationType: string
{
    case Email = '電子メール';
    case Sms = 'テキスト';
}
```

PHP では、Enum データ型は特殊な定数のように振る舞う古典的なオブジェクトですが、 渡すことができるインスタンスも持っています。ただし、通常のオブジェクトとは異なり、さまざまな制約を受ける。

Enumとオブジェクトの違い
-----------------------

列挙型はクラスやオブジェクトの上に構築されていますが、オブジェクトに関連するすべての機能をサポートしているわけではありません。特に、enumオブジェクトは内部状態を持つことが禁止されています（常にstaticクラスでなければなりません）。

相違点の具体的なリスト。

- コンストラクタとデストラクタは禁止されています。
- 継承には対応していません。列挙型は、他のクラスによる拡張や継承はできません。
- 静的なプロパティやオブジェクトのプロパティは使用できません。
- 特定のEnum値（インスタンス）のクローニングはサポートされておらず、個々のインスタンスはシングルトンインスタンスである必要があります。
- 以下に記載されている以外のマジックは禁止されています。

以下のオブジェクトの機能が利用でき、他のオブジェクトと同様に動作します。

- パブリック、プライベート、プロテクトの方法。
- Public、private、protected の静的メソッド。
- パブリック、プライベート、プロテクト定数
- 列挙型は、任意の数のインターフェースを実装することができます。
- 属性は、列挙型とケース型に付けることができる。ターゲットフィルター `TARGET_CLASS` は Enum 自体を含む。ターゲットフィルター `TARGET_CLASS_CONST` は Enum のケースを含みます。
- マジックメソッド `__call`、`__callStatic` および `__invoke` を使用します。
- 定数 `__CLASS__` と `__FUNCTION__` は通常の定数のように振る舞います。
- Enum 型のマジック定数 `::class` は、オブジェクトの場合と同様に、名前空間を含むデータ型の完全な名前として評価されます。型 `Case` のインスタンスに付けられたマジック定数 `::class` は、Enum 型としても評価されます。

Enumをデータ型として使用する
-----------------------------

スーツの種類を表すenumがあるとします。この場合、`Suit`型を定義して、個々の有効な値を格納するだけでよい。

そして、特定のオプションのインスタンスを、静的定数を扱うときのように、古典的に2次関数で得ます。

Enumを定義し、特定の型によって呼び出したり、関数に渡したりする例。

```php
enum Suit
{
	case Hearts;
	case Diamonds;
	case Clubs;
	case Spades;
}

function doStuff(Suit $s)
{
	// ...
}

doStuff(Suit::Spades);
```

2つの数値の比較
---------------------

オブジェクトや定数に対する列挙型の基本的な利点は、その値を容易に比較できることである。

特定の値で作業する基本的な比較は、以下のように行うことができます。

```php
$a = Suit::Spades;
$b = Suit::Spades;

$a === $b; // 真
```

また、ある特定の値が有効なEnum値の列挙に属するかどうかを判断する必要がある場合も非常に多い。これは次のように簡単に検証することができる。

```php
$a = Suit::Spades;

$a instanceof Suit;  // 真
```

型の値を読み取る
---------------------

特定の型の値を、呼び出す定数の名前として、あるいは（存在すれば）直接、実定義の値として読み取ることができるのです。

```php
enum Colors
{
	case Red;
	case Blue;
	case Green;

	public function getColor(): string
	{
	    return $this->name;
	}
}

function paintColor(Colors $colors): void
{
	echo "ペイント" . $colors->getColor();
}
```

呼び出し元の定数の値は `name` プロパティを介して読み込まれます。また、Enumデータ型に直接カスタム関数を実装し、各Enumの上で呼び出すことができることも重要である。

特定のEnumが実数値も実装している場合（各定数の下に隠されている）、その値も読み取ることができる。

```php
enum OrderNotificationType: string
{
    case Email = '電子メール';
    case Sms = 'テキスト';
}

$type = OrderNotificationType::Email;

echo $type->value;
```

すべての有効なEnum値
-----------------------------

しばしば、Enumが取り得るすべての値をリストアップする必要があります（例えば、エラーメッセージの中でユーザーに対して）。定数を使用する場合、これは不可能でしたが、Enumを使用すると簡単に実現できます。

```php
Suit::cases();
```

Suit::Hearts, Suit::Diamonds, Suit::Clubs, Suit::Spades]` を返します。

変数がEnum型であることを確認する。
---------------------------------

特定の未知変数にEnumが含まれているかどうかは、条件によって簡単に確認することができる。

```php
if ($haystack instanceof \BackedEnum) {
```

各Enumオブジェクトは、自動的に一般的な `BackedEnum` インターフェースの子オブジェクトになります。

詳しくはPhpStan GitHubのディスカッションをご覧ください： https://github.com/phpstan/phpstan/issues/7304
