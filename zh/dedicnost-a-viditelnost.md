个人防护设备的遗传性和可见性
==============

> id: '71896a55-dcfd-4bb6-9f53-64f22b1514eb'
> slug:
> 	cs: dedicnost-a-viditelnost
> 	zh: ge-ren-fang-hu-she-bei-de-yi-chuan-xing-he-ke-jian-xing
> 
> publicationDate: '2020-02-16 22:17:05'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '4584f3dcfdcfdc506d34a37c36fe6dd1'

面向对象编程的基本属性之一是**继承**和<a href="/encapsulation">封装</a>。有了这些功能，你将能够轻松地构建复杂的应用逻辑，同时保持良好的实现可读性。

继承的原则
-------------------

继承表达了一个类的实现是基于另一个类的。在OOP术语中，我们谈论**后裔**（继承的类）和**祖先**（我们继承的类）。

一般来说，继承的工作方式是，子代获得祖代的所有特性，或者完全采用祖代的特性，或者以自己的方式修改它们，或者完全覆盖它们并使用自己的实现。

这种方法的使用非常广泛，许多<a href="/design-patterns">设计模式</a>都使用了继承。

继承的真正用途--应用程序中的展示者
--------------------

继承很适合设计所谓的**呈现器，这是一种特殊的类，代表**MVC**设计模式中的链接逻辑。

例如，我们有一个由 "主页"、"联系 "和 "登录 "组成的三个页面。

随着每个页面的实现，许多逻辑将被重复（例如，接受请求，建立URL，渲染模板，并提交产生的HTML）。因此，用这种逻辑实现一个单一的祖先，并在后代中使用它就很方便。

我们首先定义祖先（类的名称并不重要，我使用的是Nette框架的惯例）。

```php
abstract class BasePresenter
{
   public function link(string $route, array $params = []): string
   {
      // 实现构建URL的方法
   }

   public function renderTemplate(string $path, array $params = []): string
   {
      //模板渲染逻辑
   }
}
```

在定义类的时候，我使用了新的`abstract`关键字，它说`BasePresenter`类是抽象的。这意味着我们不能创建它的实例，只需要使用它，让另一个类继承并实现它。抽象还有其他有用的优点，我们将在后面讨论。一个类不一定非得是抽象的才可以继承--这只是其中一种可能的设置。

现在我们可以实现第二个类，比如说`HomepagePresenter`。

```php
final class HomepagePresenter extends BasePresenter
{
   public function run(): void
   {
       // 渲染逻辑
       $this->renderTemplate('主页', [
          '联系链接' => $this->link('联系方式：默认'),
       ]);
   }
}
```

现在你有一个可以工作的`HomepagePresenter`类。请注意，这个类是 "最终 "的，这意味着它不能再被继承，保证了这些方法将被完全按照我们指定的方式使用。

当我们实现这个类时，我们创建了一个新的`run()`方法，只有`HomepagePresenter`可以处理。在该方法中，我们调用`renderTemplate()`方法和`link()`，该类并不包含这些方法。但这并不重要，因为`extends`关键字告诉我们这些方法将从哪里继承，所以这些方法被使用。

由于有了继承，我们能够实现代码的可重用性，因为一旦写好，方法就可以在多个地方使用。

重写一个特定方法的实现
------------

很多时候，在继承过程中覆盖一个特定方法的行为是非常有用的。例如，如果我们想改变`ContactPresenter'中前一个例子中的`link()'方法的行为，它将看起来像这样。

```php
final class ContactPresenter extends BasePresenter
{
   public function run(): void
   {
      // 渲染逻辑
      echo $this->link('主页：默认', []);
   }

   public function link(string $route, array $params = []): string
   {
      return 'https://baraja.cz';
   }
}
```

要覆盖实现，只需在子程序中再次定义该方法，并覆盖该方法主体。重要的是要满足接口并实现相同的输入参数。

继承的可见性
--------------------------

有时我们想在继承过程中隐藏一些方法，只在内部使用。或者，只允许它们在继承过程中使用，而不是作为一个公共接口。

因此，一般来说，有一些简单的可见性规则。我们用 "public"、"protected "或 "private "来表示方法，可见性规则如下。

- 任何人都可以从任何地方调用`public`方法，也就是说，在创建祖先和后代的实例时。
- 只有祖先或后代可以调用 "受保护 "的方法，但在创建实例时不能从公共接口调用这些方法。这些是实现继承的内部方法（一个好的用例是在前面的例子中的`link()`方法上）。
- 只有当前类可以调用`private`方法，而不考虑继承和公共接口设置。

在运行时改变可见性
----------------------------

在非常特殊的情况下，在运行时改变一个方法的可见性，然后调用它，可能是有用的。例如，不同的Doctrine库都使用了这个方法。

为了改变可见性，我们使用由PHP自己实现的本地**反射类**类。
