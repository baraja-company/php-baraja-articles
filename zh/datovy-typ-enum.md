PHP中的数据类型Enum对象
===============

> id: '5b96765c-16c2-4f76-8e31-6de6e9f59365'
> slug:
> 	cs: datovy-typ-enum
> 	zh: php-zhong-de-shu-ju-lei-xingenum-dui-xiang
> 
> publicationDate: '2022-05-29 15:30:00'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: '43735e926db87d2cc6c538ddb67e0a69'

从 PHP 8.1 开始，Enum 数据类型可以用来为一个列表定义精确的枚举值。这对于我们知道一个变量的值只能采取特定的几个值的情况很有用。

例如，我是这样存储通知类型的。

```php
enum OrderNotificationType: string
{
    case Email = '电子邮件';
    case Sms = '文本';
}
```

在PHP中，Enum数据类型是一个经典的对象，它的行为就像一个特殊类型的常量，但也有一个可以被传递的实例。然而，与普通对象不同，它受到一些限制。

枚举和对象之间的区别
-----------------------

尽管枚举是建立在类和对象之上的，但它们并不支持与对象相关的所有功能。特别是，枚举对象被禁止拥有内部状态（它们必须始终是一个静态类）。

一个具体的差异清单。

- 禁止构造器和析构器。
- 不支持继承。枚举不能被其他类扩展或继承。
- 静态或对象属性是不允许的。
- 不支持克隆特定的枚举值（实例），每个单独的实例必须是一个单子实例。
- 除下文所述外，禁止使用魔法方法。

以下对象的特征是可用的，其行为与其他对象一样。

- 公共、私人和受保护的方法。
- 公有、私有和受保护的静态方法。
- 公有、私有和受保护的常量。
- 枚举可以实现任何数量的接口。
- 属性可以附加到枚举和案例上。目标过滤器`TARGET_CLASS`包括Enums本身。目标过滤器`TARGET_CLASS_CONST`包括Enum情况。
- 神奇的方法`__call`，`__callStatic`和`__invoke`。
- 常量"__CLASS__"和"__FUNCTION__"的行为与普通常量相同
- Enum类型上的魔法常量`::class`被评估为数据类型的全名，包括任何命名空间，与对象完全一样。在`Case`类型的实例上的魔法常量`::class`也被评估为Enum类型，因为它是不同类型的实例。

使用Enum作为数据类型
-----------------------------

想象一下，我们有一个代表诉讼类型的枚举。在这种情况下，我们只需要定义`Suit'类型并存储各个有效值。

然后我们通过二次函数得到一个特定选项的实例，就像处理一个静态常数一样。

定义一个枚举的例子，通过一个特定的类型来调用它，并把它传递给一个函数。

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

两个数值的比较
---------------------

与对象和常量相比，枚举的根本优势在于可以很容易地比较它们的值。

一个基本的比较，我们用一个特定的值来工作，可以做如下的比较。

```php
$a = Suit::Spades;
$b = Suit::Spades;

$a === $b; // true
```

很多时候，我们也需要决定一个特定的值是否属于一个有效的Enum值枚举中。这可以很容易地得到验证，如下所示。

```php
$a = Suit::Spades;

$a instanceof Suit;  // true
```

读取类型的值
---------------------

我们可以将一个特定的类型值作为一个调用常量的名称来读取，或者直接作为一个真实的定义值来读取（如果它存在的话）。

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
	echo "油漆。" . $colors->getColor();
}
```

调用常数的值是通过`name`属性读取的。同样重要的是，自定义函数可以直接在Enum数据类型中实现，它可以在每个Enum上调用。

如果一个特定的Enum也实现了实值（隐藏在每个常量之下），它们的值也可以被读取。

```php
enum OrderNotificationType: string
{
    case Email = '电子邮件';
    case Sms = '文本';
}

$type = OrderNotificationType::Email;

echo $type->value;
```

所有有效的枚举值
-----------------------------

通常我们需要列出（例如，在错误信息中向用户说明）Enum的所有可能的值。当使用常量时，这是不可能的，Enum允许这样做，很容易。

```php
Suit::cases();
```

返回 `[Suit::Hearts, Suit::Diamonds, Suit::Clos, Suit::Spades]`。

验证该变量是否为Enum类型
---------------------------------

我们可以通过一个条件轻松地验证某个未知变量是否包含一个Enum。

```php
if ($haystack instanceof \BackedEnum) {
```

每个枚举对象都自动成为通用的`/BackedEnum'接口的子女。

欲了解更多信息，请参见PhpStan GitHub上的讨论：https://github.com/phpstan/phpstan/issues/7304
