PHP 8出炉了--完整概述
==============

> id: '8b6ce751-195f-41d2-82c6-1af4be3e86b5'
> slug:
> 	cs: php-8-kompletni-prehled-novinek
> 	zh: php-8chu-lu-le-wan-zheng-gai-shu
> 
> publicationDate: '2020-11-26 11:53:54'
> mainCategoryId: '17545205-215b-4962-b910-0d67ad1e933a'
> sourceContentHash: f657145ac1d2a1109fcc2a1ff3b6e6cf

今天，2020年11月26日，PHP 8的新的主要版本在几年后发布，它包括一系列大胆的新功能。这是很长一段时间以来最大的更新之一，值得专门写一篇文章。

在这篇文章中，我们将总结所有主要的新功能，以及与旧版本相比在语法和选项上的差异。大多数新功能是向后兼容的，并带来了行为上的改进，你会喜欢。

> **重要信息：** PHP 8 现在处于 "功能冻结 "阶段，这意味着不能再添加新的行为，只能修复错误。因此，你可以依靠兼容性，充分调试你的应用程序。

联盟类型
----------

近年来，PHP总体上已经从一种纯粹的动态语言，即任何变量都可以包含任何东西，转变为一种严格的形式，即我们事先知道什么数据类型将出现在什么变量、参数、参数或属性中。使用[data-types](/datove-types)仍然是可选的，但我推荐使用强类型，我自己在所有项目中都使用。


联盟类型表达了多个类型的集合，接受其中的任何参数或属性。

比如说。

```php
function validatePsc(string|int $psc): bool
{
	// 执行
}
```

`$psc`变量中的`validatePsc()`函数接受`string`（字符串）和`int`（整数）数据类型。

在以前的PHP7.4版本中，这种符号是不可能的，通常是通过[comment](/document-comment)来绕过。

```php
/**
 *@param string|int $psc
 */
function validatePsc($psc): bool
{
	// 执行
}
```

然而，这个注解注释被PHP忽略了（毕竟是个注释），我们不得不用PhpStan这样的外部工具进行额外的检查，而很多开发者都忽略了这一点。现在，检查是在运行时直接进行的（当应用程序运行时），不能被绕过。

然而，PHP从第7版开始就知道某种类型的联合类型，当时可以说主类型也可以是`nullable`，即它接受主数据类型加上值`null`。

这句话有两种写法，每一种都有不同的含义。

```php
function setPhone(?string $phone): void
{
	// 执行
}

// 或

function setPhone(string $phone = null): void
{
	// 执行
}

//或组合

function setPhone(?string $phone = null): void
{
	// 执行
}
```

所有条目都说电话`int`（整数）是`string`或`null`。

- 第一个符号总是要求传递值
- 第二个符号不需要传递任何值；如果没有传递任何值，默认值是`null`（这是一个可选参数）。
- 第三个条目是选项的组合，其行为类似于第二个条目

当使用联合类型时，我们将不能再使用带问号的符号，必须严格定义`null`数据类型，例如。

```php
function setPhone(string|int|null $phone = null): void
{
	// 执行
}
```

电话号码现在必须是`string`、`int`或`null`。

联盟类型仍然有许多用途，高级开发者会在特定库的文档或实现中读到这些用途。


JIT--更快的脚本处理
----------------------------------

JIT（just in time）编译器带来了脚本复杂化（解析和理解）性能的显著改善。然而，这种行为在网络请求的背景下可能会有所不同。

现在你可以在Nette框架内的Tracy栏中看到你是否启用了JIT，更多细节请参见[独立文章]（https://stitcher.io/blog/php-jit）。

关于编译，一般来说，可以说 PHP 试图在前面处理代码，这样当它处理一个特定的请求时，它就不必去看物理脚本文件，解析它，并解释它。在过去，这是通过OPCache扩展来处理的（服务器和主机默认都有这个扩展），它的处理速度提高了一半左右。

作为一般的经验法则，如果你有一个缓慢的应用程序，选择一个合适的算法来处理一个特定的任务总是比在代码中进行微优化要好。通常情况下，大的延迟是由等待数据库及其缓慢的查询、存储会话、等待硬盘空间变得可用以及其他硬件操作造成的。

不安全运算符（可选择链式）。
-------------------------------------

在实际应用中，经常需要验证一个方法的返回值的存在（它不是 "空"），然后有条件地调用另一个。三元运算符](/ternary-operator)在这方面很好，但它们只对一个条件起作用，而且不能嵌套。nullsafe操作符允许本地嵌套。

> **TIP:** 几乎相同的行为已经被Latte模板系统所支持，但它在本地PHP代码中覆盖了这种类型的语法，所以你可以在旧版本的PHP（从PHP 7开始）上使用nullsafe操作符。 为David的这个修改点个赞!

这使得它易于使用。

```php
$orderId = $order?->getId();
```

`$orderId`变量包含由`getId()`方法返回的值，或者如果`$order`变量包含值`null`且`getId()`方法不能被调用，则`null`。

这类问题在PHP7中通过三元操作符的以下语法得到了规避。

```php
$orderId = isset($order) ? $order->getId() : null;
```

可能是一个条件。

```php
if (isset($order)) {
	$orderId = $order->getId();
} else {
	$orderId = null;
}
```

该条目可以进一步写入调用。我从[Latte documentation](https://latte.nette.org/cs/syntax#toc-volitelne-retezeni-s-nullsafe-operatorem)中提取了样本，它的描述非常完美。

```php
$orderName = $order->item?->name;

//与此相同。

$orderName = isset($order->item) ? $order->item->name : null;
```

典型的用法是在模板中列出更复杂的结构，例如在Latte中，它看起来像这样（样本取自文档）。

```php
{$user?->address?->street}
//意味着大约($user !== null) &&($user->address !== null) ?$user->address->street : null

{$items[2]?->count}
// 替换大约（$items[2] !== null）？$items[2]->count : null

{$user->getIdentity()?->name}
// 替换大约 $user->getIdentity() !== null ?$user->getIdentity()->name : null
```

在真正的代码中，它可以是这样的，例如，我们想通过阅读客户的资料来找出他的国家（而且你有数据库中的数据通过会话很好地存储，因为它应该这样做），那么在旧的PHP中它看起来是这样的。

```php
$country =  null;
if ($session !== null) {
    $user = $session->user;
    if ($user !== null) {
        $address = $user->getAddress();
        if ($address !== null) {
            $country = $address->country;
        }
    }
}
```

现在，它可以缩短为一行。

```php
$country = $session?->user?->getAddress()?->country;
```

使用nullsafe操作符还可以防止在PHP7中没有经验的开发者不容易发现的各种bug。

例如，这个条目将产生一个致命的错误。

```php
var_dump($invoice->getDate()->format('Y-m-d') ?? null);

//返回：致命错误：未捕获错误：对null调用成员函数format()。
```

正确的语法如下。

```php
var_dump($invoice->getDate()?->format('Y-m-d'));

//返回：null
```

命名的论据
---------------------

在古老的PHP中，带参数的函数调用必须按照目标函数定义的准确顺序传递参数来编写。这样做没有错，但是，当使用一些数值相似的参数时，可能导致可读性变差。或者，如果我们想传递到顺序中的第n个参数，那么所有的可选参数都必须在前面传递，这可能会对可读性和向前兼容产生负面影响。

想象一下，例如Nette中的`setCookie()`函数，它有很多参数。

```php
public function setCookie(
	string $name,
	string $value,
	$time,
	string $path = null,
	string $domain = null,
	bool $secure = null,
	bool $httpOnly = null,
	string $sameSite = null
)
```

前三个参数（`$name`、`$value`和`$time`）是强制性的，但如果我们想传递`$httpOnly`参数，我们必须传递前面所有的参数并正确计算顺序。

```php
$http->setCookie(
	'我的Cookie',
	'大卫喜欢马',
	'现在',
	null, // 路径
	null, // 域名
	null, // 安全
	true
);
```

如果不是必须这样做，你根本不想这样做。

优雅的写作，然后看起来像。

```php
$http->setCookie(
    name: '我的Cookie',
    value: '大卫喜欢马',
    time: '现在',
    httpOnly: true
);
```

这种类型的语法要求目标函数中的参数名称永远不要改变，因为它们在被调用时仍将被打入。至少开发者将能够更好地命名它们。

如果我们只想使用其中一个参数，语法可以合并并浓缩为一行。

```php
$http->setCookie('我的Cookie', '大卫喜欢马', '现在', httpOnly: true);
```

前3个参数以原来的方式传递，然后传递可选的参数`httpOnly`（因为它被命名了）。

属性
---------

大多数主要语言，如Java或C#，已经原生包含了所谓的注解，这是一种原生语言语法，允许你为其他语言结构添加元信息。

在PHP中，这种类型的语法长期以来一直缺失，并通过使用DOC注释来规避，这是一个经典的方法注释，只不过它有两个`/**`星号。

这些注释在脚本处理过程中被忽略，必须添加特殊的用户逻辑，以便在运行时通过反射来解析和解释它们。你也许可以理解这对性能的影响，再加上注释的语法不能被要求，而且在编译时（当脚本在运行前被处理时）很难检查，而且你必须使用正常PHP工具包之外的额外工具来做这件事。

为了保持向后的兼容性，PHP提供了类似于备用注释符号的语法的属性，这不会破坏在传统的PHP上运行脚本。

原来的记号（例如，用于Nette Presenter中的Inject依赖）。

```php
final class HomepagePresenter extends BasePresenter
{
	/** @inject */
	public EntityManager $entityManager;
}
```

现在你可以删除注释并使用本地属性。

```php
use App\Attributes\Inject;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

非常重要的是，属性不再只是注释中的一段字符串，而是一个有效的PHP代码的物理类。

这很好，因为你现在可以安全地验证一个属性的输入，而且使用一个属性实际上变成了对其构造函数的调用，可以使用其他逻辑。我很期待看到Doctrine能原生地支持这一点，因为它对一切都使用注释。

然后，属性本身的实现可能看起来像这样。

```php
#[Attribute]
class Inject
{
    public string $value;

    public function __construct(string $value)
    {
        $this->value = $value;
    }
}
```

严格的逻辑可以在属性内再次使用，如检查参数数据类型、联合类型和其他语言特征。

匹配表达式
----------------

新的语言结构`match()`是对老式`switch()`的现代化改进（我尽量不使用它），并带来了许多很酷的功能（这将使我再次开始使用它）。

例如，我们想根据输入修改一个变量的值。

```php
$pozdrav = match(bool $formal) {
    true => '你好',
    false => '尊敬的先生，您好',
};
```

语法的一个重要的新特点是，我们不必使用`break`（像以前的`switch`），一般来说，语法更经济。

同时，我们可以在一个条件中同时验证多个输入（用逗号分隔），并可能返回一个默认值（当没有满足时）。

例如，在将HTTP条件代码改写为错误信息时，这就很方便了（在处理异常代码时，你肯定会欣赏它）。

```php
$message = match ($statusCode) {
    200, 300 => null,
    400 => '未找到',
    500 => '服务器错误',
    default => '未知状态代码',
};
```

值的比较是由`====`运算符严格完成的（switch只使用`==`），这再次表明PHP遵循了严格的设计路线。因此，在前面的情况下，输入的`'200'（一个包含数字的字符串）将不被接受。

如果你没有为 "default "指定一个值，并且没有匹配，就会抛出一个 "未处理的匹配错误"。

新的语法还允许使用表达式或函数调用进行匹配（它的行为类似于条件）。如果出现错误，我们就可以抛出一个异常（因为`throw`标记现在是一个表达式，可以这样使用）。

```php
$message = match ($statusCode) {
    200 => null,
    $this->checkServerError($statusCode) => throw new ServerError(),
    default => '未知状态代码',
};
```

传播到构造函数的属性
-------------------------------------------------

这只是一个语法糖，对于直接在构造函数中快速简单地定义一个实体及其属性非常有用。


例如，原始实体。

```php
final class User
{
    public string $name;

    public function __construct(
        string $name,
    ) {
        $this->name = $name;
    }
}
```

它只能被缩短为。

```php
final class User
{
    public function __construct(
        public string $name
    ) {}
}
```

`$name`属性是根据`string`数据类型验证的，它的值可以直接从实例中读取，因为它是一个公共属性。此外，如果你在Nette中使用SmartObject（我相当不建议在PHP8中使用），你可以通过先调用它们的getter方法来访问私有属性，这种语法又简化了事情。

返回类型静态
--------

在过去，我们可以使用 "self "数据类型作为方法的返回值，但它会返回一个定义它的类的实例。`静态`数据类型通常可以做到这一点，即使在继承的情况下，也会返回执行实例的类的数据类型，而不是其祖先。

比如说。

```php
class BaseEndpoint
{
    public function getInstance(): static
    {
        return new static();
    }
}
```

数据类型混合
------------------

混合 "类型现在可以作为一个函数或方法的参数使用。这意味着该方法必须始终接受一些输入（因此是一个强制性参数）。

如果你至少能做到一点，总是使用直接的数据类型，或者至少是union。只有当函数真的接受任何东西时，混合才是有用的。在实践中，这种用法很有用，例如，对于各种接受任意输入并必须能够显示的转储函数。

混合类型接受以下类型："string", "int", "float", "null", "bool", "array", "callable", "object", "resource"。

然后，David将使用混合类型来实现他的功能。

```php
function bdump(mixed $var): mixed
{
	Tracy\Debugger::barDump($var);
	return $var;
}
```

作为表达式的代币抛出
------------------------

`throw`标记现在变成了一个表达式，这意味着在实践中，当`fn()`lambda函数被截断时，或者当三元运算符被检查时，可以抛出一个异常。

```php
$error = fn () => throw new \InvalidArgumentException('这总是抛出一个错误。');

$userName = $user['名称'] ?? throw new \LogicException('该用户必须有一个名字。');
```

str_contains()函数
-----------------------

PHP最终包含了一个本地函数来检查默认字符串是否包含子串。

比如说。

```php
if (str_contains('洪齐克喜欢猫。', '猫咪')) {
	echo '该功能处理猫的问题。';
}
```

在过去，子串的出现是通过[strpos](/strpos-function)函数来验证的。

```php
if (strpos('洪齐克喜欢猫。', '猫咪') !== false) {
	echo '该功能处理猫的问题。';
}
```

函数str_starts_with()和str_ends_with()。
----------------------------------------------

一对新函数用于检查一个字符串是否以子串开始或结束。

```php
str_starts_with('洪齐克喜欢猫。', '鸿志克'); // true

str_ends_with('洪齐克喜欢猫。', '猫咪。'); // true
```

函数 get_debug_type()
-------------------------

增强现有[gettype](/function-gettype)函数的输出，该函数只返回所传递变量的通用类型。例如，当抛出一个异常时，当我们得到非有效的输入并想告诉用户他们实际传递了什么时，该函数就被使用。

当我们用一个包含``App\User`类实例的变量调用`gettype()`函数时，该函数返回`object`，所以我们不知道它是什么类。新函数`get_debug_type()`返回类的名称。

函数get_resource_id()
--------------------------

该函数从一个变量中返回一个外部资源的标识符。

例如，连接到MySql数据库是由PHP通过使用一个特殊的`resource`数据类型来处理的，现在有可能找出分配给它的ID。

> **历史性的说明：**
>
> PHP中的 "resource "类型是在它还不知道如何使用对象的时候创建的，它不得不想办法解决如何传递对类似 "data "类型的引用。在未来，你可以期待`resource`被完全从语言中移除，所以最好不要使用这个功能。

ext-json扩展总是可用的
-------------------------------------

在过去，PHP可以在不支持json的情况下进行编译。现在，json总是可用的，所以你可以从你的`composer.json`文件中删除`ext-json`依赖，并且总是知道可以使用json。

串联的优先级
-------------------------------------------------------

想象一下，像这样的事情。

```php
echo '总的来说。' . $a + $b;
```

是先做数字加法，还是先将变量`$a`附加到字符串中，然后将整个新字符串加到`$b`中？

人们会期望先做加法，但这是一个很好的假设。PHP实际上是这样做的。

```php
echo ('总的来说。' . $a) + $b;
```

现在 PHP 8 的行为将是可预测的。

```php
echo '总的来说。' . ($a + $b);
```

然而，一般来说，使用括号来限定一个表达式总是更好的。

稳定的排序
---------------

在PHP 8之前，字符串排序是使用所谓的不稳定算法，这意味着PHP不能保证具有相同（或其他相等）值的元素不会被调换。新版本将所有排序函数的行为改为稳定，因此排序总是以确定的方式进行，你总是得到相同的输出。

例如，这就解决了我们按相关性对用户评级进行排名的情况，但一些评级具有相同的分数。现在，每次排序时，它们将以相同的顺序出现，不会连续跳过。

其他新功能
---------------

PHP还有许多其他小的新功能和改进。例如，错误会以不同的方式抛出（但这并不影响我们这些写无错误代码的人，对吗？）

你可以随时在官方文档和RFC帖子中看到完整的变化列表。

我在新的PHP中错过了什么
-----------------------

我希望PHP最终能支持复合数组类型，例如当一个方法返回一个标识符的数组时，我们仍然只能指定`getIds(): array`，而像`getIds(): int[]`这样的东西会好很多。也许我们很快就会看到这一点，强类型检查就会完成。

更多资源
------------

David Grudl就Posobot的新东西做了一个很好的演讲。我建议观看录音。

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/ZYkwmcCUTCg" frameborder="0" allow=" accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

这是为了感谢David的讲座，因为我从他的讲座中汲取了一些信息用于这篇文章。特别是关于Nette转向PHP 8的东西和其他关于PHP的幕后技巧。
