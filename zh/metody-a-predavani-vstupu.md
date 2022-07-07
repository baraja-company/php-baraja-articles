PPE和投入转移的方法
===========

> id: '843fbfb4-daf2-4c2e-9d94-28d494025b2e'
> slug:
> 	cs: metody-a-predavani-vstupu
> 	zh: ppe-he-tou-ru-zhuan-yi-de-fang-fa
> 
> publicationDate: '2020-02-16 20:49:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3e1bca690ed70479ef9807a1f2a1f23

方法代表了一个对象的行为，因为它们允许你处理其内部状态，以及影响对象之间的关系。

在现实世界中表示方法
----------------------------------

考虑任何现实世界的物体，例如一只猫。一只猫有某些属性（名字、颜色、体重......），我们用属性来描述，然后还有行为（喵喵叫、走路、睡觉......），我们用方法来描述。由于世界上有很多猫（"于是我像打雷一样吠叫，让有一百万只猫"），重要的是要记住，方法是一种普遍的东西，同样适用于所有特定类型的对象。

实际执行一种方法
-----------------------------

在语言语法方面，让我们为一只猫定义一个类。

```php
class Cat
{
    public string $name;

    public string $sound = '喵喵';
}
```

在创建了某只猫的实例后，我们可以简单地列出，比如说，声音。

```php
$cat = new Cat;

echo $cat->sound; //说 "喵"。
```

但是，如果我们想在写出声音时以某种方式进行格式化呢？然后，方法就开始发挥作用。

```php
class Cat
{
    public string $name;

    public string $sound = '喵喵';

    public function getFormattedSound(): string
    {
        return '我在发声"' . $this->sound . '"!';
    }
}
```

你必须已经知道过去的功能原理。类允许将一个函数直接写入其主体，并定义了可访问性（与属性一样），被称为方法。

`$this`变量的行为有点 "神奇"。它存储了我们当前所处的对象的当前实例。如果我们想从一个属性中读取一个值或者在一个方法中调用另一个方法，我们只需要在`$this`变量上进行操作。

然后，该方法从对象内部作为一个经典的函数被调用（结尾处有一个括号），表示它不是一个属性。

```php
$cat = new Cat;

echo $cat->sound; //说 "喵"。

echo $cat->getFormattedSound(); //打印 "我在发出'喵'的声音！"。
```

构造函数 - 创建实例时调用的方法
--------------------------------------------------

在创建一个对象实例时，我们经常需要定义如何设置它的基本状态以及哪些参数（输入数据）是必须的。

在OOP中，为了解决这个问题，有一个特殊的公共方法`__construct`，我们可以自愿实现，它总是而且只在创建一个实例时被调用。

实际例子。

```php
class Cat
{
    public string $name;

    public string $sound;

    public function __construct(string $name, string $sound)
    {
        $this->name = $name;
        $this->sound = $sound;
    }

    public function getFormattedSound(): string
    {
        return '我在发声"' . $this->sound . '"!';
    }
}
```

通过定义构造函数，我们确保了在创建实例时，我们总是要传递2个强制性参数（`name'和`sound'），对象将被直接设置。

由于构造函数是一个经典的方法，它可以在里面对插入的数据做其他的事情。例如，对字符串进行适当的重新格式化，但我们以后会处理这个问题。

然后创建一个实例又很容易了，我们只需要传递初始参数（它们写在类名的括号里，在类名被调用的地方，实例被创建）。

```php
$cat = new Cat('敏达', 'ǞǞǞ');

echo $cat->name; // 上面写着 "Minda"
echo $cat->sound; //打印出 "Vrr"。

echo $cat->getFormattedSound(); //打印 "我在发出 "Vrr "的声音！
```

纪特利和纪特利
-----------------

有些方法被称为 "getters "和 "setters"。这些都是常见的方法，只是约定俗成地这样称呼它们而已。方法是用来获取和插入数据到一个对象。主要的优点是，我们可以在插入数据之前对其进行验证，并可能对其进行修正，或者拒绝数据并抛出一个错误。

对于每一个可以被检索的属性（我们想实现数据检索），习惯上是创建一个以 "get "开头的getter。如果该属性返回一个布尔值（"true "或 "false"），习惯上用 "is "命名方法名称的开头。

简化的例子。

```php
class Cat
{
    public string $name;

    public function __construct(string $name)
    {
        $this->setName($name);
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function isEmpty(): bool
    {
        return $this->name === '';
    }

    public function setName(string $name): void
    {
        $this->name = trim($name);
    }
}
```

当创建一个猫的实例时，我们把它的名字传给构造函数（这总是强制性的）。然而，由于我们希望构造函数和setter（插入新名称的方法）共享插入名称的逻辑，我们在构造函数中调用`setName()`方法，以确保名称被插入。

在setter里面，我们使用<a href="/function-trim">trim()</a>函数，它可以自动删除插入的字符串两边的空白。

我们可以使用`getName()`方法进行输出。`isEmpty()`方法可以用来检查空性。

> **实用的优势：**。
>
> getters和setters的一个巨大优势是**保证的数据类型**。如果一个getter声称要返回一个 "字符串"，我们总是可以依赖它，并且总是得到一个字符串。
>
> 一个setter在其实现中也声称接受一个`string'，这对应用程序来说意味着无论用户输入什么都会被转换为`string'（如果它使用兼容的类型，如`integer'）或者得到一个错误，即该类型不能被转换（如`array'）。

获取器和设置器的最大附加价值将在实际使用中得到特别赞赏。

神奇的方法`__toString()`。
-----------------------------

在一个类中，你可以定义一个`__toString()`魔法方法，当你试图把一个对象写成字符串时，它会被自动调用。该方法必须总是返回一个字符串。

实施实例。

```php
class Cat
{
    public string $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function __toString(): string
    {
        return '你好，我是' . $this->name . ';)';
    }
}
```

然后，使用情况如下。

```php
$cat = new Cat('敏达');

echo $cat; // 它说："嗨，我是明达；）"
```

摘要
-------

我们已经展示了如何定义方法，然后在一个对象实例中调用它们。

下一次，我们将看看<a href="/encapsulation">封装原则</a>，它充分利用了方法的属性。
