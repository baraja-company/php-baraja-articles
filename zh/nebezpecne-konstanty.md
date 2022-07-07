在PHP中不安全地使用常量
=============

> id: b4d0f45f-c059-4a47-9b63-8980d0d465ed
> slug:
> 	cs: nebezpecne-konstanty
> 	zh: zaiphp-zhong-bu-an-quan-de-shi-yong-chang-liang
> 
> publicationDate: '2021-06-22 10:49:46'
> mainCategoryId: '561b0614-e152-4a62-a634-1f2d605d39d9'
> sourceContentHash: '58d4ea2e4bf106402386506b023ba4d2'

在PHP中使用常量时，有两件棘手的事情需要注意。

动态和静态常数
------------------------------

常量可以在PHP中直接静态地定义在类中（最好的解决方案），例如，如下所示。

```php
class Region
{
	public const PREFIX = 420;
}
```

而且用途相当明确。在类编译时，常量的值已经决定了，我们可以通过调用类名和常量本身来访问它。最常见的是通过写`Region::PREFIX`。

另一种（更糟糕的方式）是在运行时动态地定义常量（通常是在配置脚本中的某个地方），这时就会出现这样的情况。

```php
define('BASE_DIR', __DIR__ . '/../');
```

通过`define`函数定义常量的主要缺点是，定义常量的脚本可能没有被调用，所以当你试图读取常量时，常量将不存在。

当与类中静态常量定义中的动态常量结合使用时，这甚至会导致致命的反射错误。

```php
class InvoiceGenerator
{
	// 这是完全错误的!
	public const DATA_DIR = BASE_DIR . '/data/invoice';
}
```

> **解释：**
>
> 在静态常量中使用动态常量有一个主要的缺点，即动态常量的值在编译时不能被读取。因此，这个脚本必须在每次请求中再次处理（即不能为了优化速度而存储在OPCache中），如果这个常量根本不存在，就会出现致命的编译错误，应用程序根本无法运行。

如果你使用PhpStan，它可以自动警告你这个问题。

```txt
Reflection error: Could not locate constant "BASE_DIR" while
evaluating expression in InvoiceGenerator at line 6
```

> **学习：**
>
> 所有常数的值都应该是常数。


使用静态时常量的继承性
-------------------------------------

在某些情况下，使用继承来覆盖一个常量的值是有意义的。但在这种情况下，祖先不能从后代那里读取值（或不应该）。

一个例子是在定义国家和地区的时候。

```php
abstract class Region
{
	public function getPrefix(): int
	{
		// 致命的错误!
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

矛盾的是，上述代码不一定会抛出错误，但却会因为不恰当地使用继承而抛出错误。

如果我们在 "CzechRepublic "的后裔上调用 "getPrefix() "方法，一切都将是正确的，因为常数的值将被正确读取。然而，如果子代没有设置常量的值，将抛出一个致命的不存在常量的错误。整个事情最糟糕的部分是，这是一个**隐藏的依赖关系，在方法的实现中被创建，而继承这个类的开发者可能根本不知道这个依赖关系。

在这种情况下，最好的解决办法是直接在祖先中定义一个带有默认值的常量（这样逻辑就会一直通过），或者至少在getter中抛出一个异常。

```php
abstract class Region
{
	public const REGION = null;

	public function getPrefix(): int
	{
		if (static::REGION === null) {
			throw new \LogicException('区域还没有被定义。');
		}
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

PhpStan对这个错误的反应如下。

```txt
Access to undefined constant static(Region):REGION.
```
