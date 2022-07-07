在PHP中自动加载类
==========

> id: f6cd5762-261f-4153-b27b-075dd8b5ed13
> slug:
> 	cs: autoloading-trid
> 	zh: zaiphp-zhong-zi-dong-jia-zai-lei
> 
> publicationDate: '2020-02-09 10:00:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '441a1d4107bb32e8cfe4dbb926c2decd'

我相信你知道这一点，在为PHP脚本编程时，我们把代码分成许多文件，为了让所有的部分都能使用，我们用一系列的 "include"、"require "或最好是 "require_once "调用来加载它们，这样可以保证只加载一次。

在代码中，它看起来像这样。

```php
require_once 'Router.php';
require_once 'Page.php';
require_once 'Paginator.php';
```

这种方法的主要缺点是，程序员必须不断地确保所有的东西总是被加载。但是，如果他加载很多，就会不必要地损失性能，并多次到达磁盘。因此，手动解决方案只有问题。

本地自动加载
-------------------

幸运的是，PHP中原有的支持所谓的**类自动加载**，这是代码中的逻辑，只在第一次需要时（通常是第一次创建实例时）加载一个类文件。

一个简单的实现可能看起来像这样。

```php
spl_autoload_register(function (string $className): void {
    include 'src/' . $className . '.php';
});

$obj  = new MyClass1();
$obj2 = new MyClass2();
```

当创建一个`MyClass1`类的实例时，`spl_autoload_register`函数从`src`目录中读取`MyClass1.php`文件。这个实现假设每个类都在一个单独的文件中，以类或接口的名称来调用。

更为复杂的自动装载案例
-------------------------------

在实际应用中，可能会出现许多令人不快的情况，使自动加载变得复杂，例如。

- 该类或接口根本不存在
- 该文件已经被加载过一次
- 在同一个文件中存在多个类或接口
- 类或接口的路径与名称不一致
- 类或接口的位置随时间而变化

然而，我们不需要为这一切编制自己的解决方案，而是可以使用现有的解决方案，一旦设计出来。

如果您正在使用<a href="https://getcomposer.org/doc/01-basic-usage.md">Composer</a>，您可能也在使用它的本地自动加载。这是因为当你安装任何软件包时，Composer会自动生成一个 "类图"，这是一个类的概述和它们的物理位置。

然后在代码的开头（通常在`index.php`），你只需使用。

```php
require __DIR__ . '/vendor/autoload.php';
```

然而，自动加载只在调用 "composer dump "命令时生成一次，所以每次应用程序发生变化时都需要重新生成自动加载。

RobotLoader - 一个优雅的开发解决方案
----------------------------------------

对于本地开发，我非常喜欢`nette/robot-loader`包，它可以自动遍历目录结构并缓存类的位置。因此，如果我们加载一个类，它首先在缓存中查找，只有当它不存在时，才会自动重新索引项目。因此，程序员根本不需要跟踪任何文件或类的位置，只需进行编程。

通过Composer进行安装。

```txt
composer require nette/robot-loader
```

功能的基本解释在<a href="https://doc.nette.org/cs/3.0/robotloader">文档</a>本身中描述。

> 类似于Google的机器人对网页的抓取和索引，RobotLoader抓取了所有的PHP脚本并记录了它在其中发现的类、接口和特征。然后，它缓存其研究结果，并在下一个请求中使用它们。所以你只需要指定浏览哪些目录，在哪里缓存。

然后，它非常容易使用。

```php
$loader = new Nette\Loaders\RobotLoader;

//添加RobotLoader应该索引的目录
$loader->addDirectory(__DIR__ . '/app');
$loader->addDirectory(__DIR__ . '/libs');

//在'temp'目录下将缓存设置为磁盘。
$loader->setTempDirectory(__DIR__ . '/temp');
$loader->register(); // 启动RobotLoader
```

设置`$loader->setAutoRefresh(true or false)`可以决定RobotLoader在遇到新类时是否要重新索引文件。这应该在生产服务器上被禁用。

现在，你再也不用处理自动加载的问题了。

综合解决方案
------------------

在开发一个真正的项目时，我使用一个综合的解决方案。

现实生活中的工作方式是，我通过Composer自动加载已安装的包（这非常有效），这就解决了`vendor`目录下所有类的加载问题。

然后，特定项目的代码被放置在`app`目录下，我在那里通过RobotLoader处理自动加载的几个类。重要的是始终保持具体的应用程序尽可能小，并尽可能地使用现成的包，这在很大程度上促进了可重复使用性。

项目的结构看起来是这样的。

```txt
/app
    Bootstrap.php <-- konfigurace
    /model
        UserForm.php <-- projektové třídy
        RegisterFactory.php
        ...
/vendor
    ... <-- knihovny
/www
    index.php <-- inicializace
```

自动加载测试
------------------------

有时可能发生的情况是，并非每个文件都能加载，你会发现问题。

对于调试，我推荐使用<a href="/get-list-of-all-loaded-files">get_included_files()</a>函数。
