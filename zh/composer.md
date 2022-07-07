作曲家--高级功能的完整概述
==============

> id: a74d8d59-91ce-4602-ad52-80cf89a647bd
> slug:
> 	cs: composer
> 	zh: zuo-qu-jia-gao-ji-gong-neng-de-wan-zheng-gai-shu
> 
> perex:
> 	- Composer je pokročilý správce balíků a závislostí pro vaše PHP aplikace. Článek popisuje jeho všechny výhody a možnosti použití.
> 	- Composer是一个先进的软件包和依赖性管理器，适用于你的PHP应用程序。这篇文章介绍了它的所有优点和用途。
> 
> publicationDate: '2020-03-10 20:18:19'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '68340d4b4d3c8a6ed143ede176fbf04e'

正如你已经知道的，[Composer](https://getcomposer.org/)是一个强大的PHP包和依赖管理器，通过它你可以一次优雅地管理数百个项目，并将一旦写好的代码同时分发到所有应用程序。

本教程作为一个详细全面的开发者指南。我们将介绍使用Composer的所有重要的高级技术，以及解释技术细节和相关的依赖性。

安装Composer
-------------------

无论哪个平台，我们都从[Composer官方网站]（https://getcomposer.org/）下载。

在内部，下载了PHP文件`composer-setup.php`，在CLI模式下运行时可以安装Composer。同样重要的是要知道，没有PHP，Composer是不能工作的，所以首先要确认你的电脑上有PHP在运行（只需要在终端机上可用）。

在Mac和Linux上，Composer在安装后立即工作，你只需要调用`composer -v`命令来快速验证Composer是否正确安装。

在Linux上，可以使用以下命令进行安装：`/usr/bin/php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"`

在Windows上，安装[Git Bash for Windows](https://gitforwindows.org/)工具是个好主意，它可以让你打开一个特定的终端，其行为几乎与Linux一样，可以在与Linux相同的环境中工作。

服务器的安装与本地环境的安装相同。只要确保你有正确的PHP版本，Composer在内部使用。

可用的命令
----------------

Composer实现了一系列的命令。

其用法是：`composer <command>`，例如：`composer update`。

1.10.0 "的概述。

| 命令 | 说明 / 意义 |
|-----------------------|----------------|
| `about` | 显示关于Composer的简要信息。
| `archive` | 用选定的Composer软件包的内容创建一个档案。
| `browse` | 在网络浏览器中打开所选软件包、作者或其他相关页面的主页。通常可能包含关于如何使用它的文件。
| `cc` | 清除Composer内部缓存中过去下载的软件包的版本。
| `检查-平台-要求` | 检查是否符合当前平台的安装要求。
| `clear-cache` | 清除Composer内部缓存。
| `clearcache` | 清除Composer内部缓存。
| `config` | 设置一个配置指令。
| `create-project` | 根据选定的软件包创建一个新的项目，并自动创建一个文件夹来放置该项目。
| `depends` | 显示哪些软件包导致所选软件包被安装。
| `diagnose` | 诊断系统，找出常见的错误。对输出的处理由开发者决定，它只是一个列表。
| `dump-autoload` |生成一个新的<a href="/autoloading-trid">autoloader</a>。
| `dumpautoload` | 生成一个新的<a href="/autoloading-trid">autoloader</a>。
| `exec` | 执行供应商的二进制文件和脚本。
| `fund` | 探讨如何制作/修改你的依赖关系。
| `global` | 允许你从`$COMPOSER_HOME`变量运行全局Composer命令。
| `help` | 打印命令的帮助。
| `home` | 在浏览器中打开一个特定软件包的主页。
| `i` | 根据`composer.lock`文件安装所有项目依赖，如果它存在并且有效的话。如果出现问题，则使用`composer.json`中的信息，`composer.lock`则恢复到原来的状态。
| `info` | 显示项目中当前安装的软件包的信息。显示所有软件包的名称，它们的当前版本和简短描述。
| `init` | 在当前目录下创建一个基本的`composer.json`函数。
| `install` | 根据`composer.lock`文件安装所有项目依赖，如果它存在并且有效的话。如果出现问题，则使用`composer.json`中的信息，`composer.lock`则恢复到原来的状态。
| `licenses` | 显示所有软件包、其版本和当前许可证的列表。
| `list` | 显示可用命令的列表。
| | `outdated` | 显示所有有较新版本可供安装且符合依赖关系的软件包的列表。对于每个软件包，将显示Composer建议安装的最新兼容版本。
| `prohibits` | 显示哪些软件包和依赖关系阻止安装所请求的软件包或版本。
| `remove` | 将软件包从`require`或`require-dev`配置部分移除。
| `require` | 在`composer.json`中添加所请求的软件包并安装。如果依赖性不能得到满足，它将恢复到原来的状态。
| `run` | 运行`composer.json`中定义的脚本。
| `run-script` | 运行`composer.json`中定义的脚本。
| `search` | 通过关键词或搜索查询来搜索软件包。
| `自我更新` | 将内部的`composer.phar`更新到最新版本。
| `selfupdate` | 将内部的`composer.phar`更新为最新版本。
| | `show` | 显示当前安装的软件包的详细信息。
| `status` | 显示手动对软件包所做的本地修改的摘要，基于与最初安装的软件包的来源的比较。
| `suggests` | 显示软件包的建议。建议可以包括各种类型的行动，如安装安全更新等。
| `update` | 根据依赖关系更新整个项目，以便它们总是由`composer.json`满足。如果成功，它会更新`composer.lock`，在那里写上当前安装的版本。
| `upgrade` | 别名为`update`。
| `u` | 别名为`update`。
| `validate` | 检查`composer.json`和`composer.lock`是否有语法错误。
| `why` | 显示哪些软件包导致当前选择的软件包被安装，包括所有的依赖关系。
| `why-not` | 显示哪些软件包和版本阻止了所选软件包或版本的安装。

创建和定义一个项目
----------------------------------

每个由Composer管理的项目都由其根部的`composer.json`文件定义，该文件定义了所有的依赖性。该文件可以为现有项目手动创建，也可以在创建项目时自动创建。

因为Composer中的所有东西都是一个包，所以项目本身可以基于一个包。因此，例如，如果你正在创建几十个或几百个非常相似的项目，把它们的基本配置和结构放到一个单独的包里，作为安装的基础，是有意义的。

这种包的一个例子是我的[Baraja Sandbox](https://github.com/baraja-core/sandbox)，它基于纯粹的Nette 3.0，并为我的[Package Manager](https://github.com/baraja-core/package-manager)增加了一个基本的依赖，我在Nette配置中使用它来管理所有项目和依赖。

然后只需用命令就可以安装沙盒。

```shell
$ composer create-project baraja/sandbox <your-project-name>
```

根据项目名称，Composer将自动创建一个文件夹来安装该项目（复制包的内容并安装依赖性）。

在`vendor`文件夹中，Composer会管理所有的包（它们的物理文件在那里），并生成一个自动加载的类，我们最好直接把它作为一行放入`index.php`。

```php
require __DIR__ . '/vendor/autoload.php';

// 应用程序代码本身
```

安装额外的软件包和依赖项
-------------------------------------

在一个功能项目中，我们可以非常容易地安装新的软件包和添加依赖性。

有2种方法可以做到这一点。

- 使用 "composer require ... "命令。
- 通过直接在`composer.json`文件的`require`部分添加一个依赖关系，然后使用`composer update`命令。

试着安装例如PackageManager：`composer require baraja-core/package-manager`，或者[Doctrine](https://github.com/baraja-core/doctrine)：`composer require baraja-core/doctrine`。

如果选择的软件包不能安装，可以询问具体的原因，Composer将列出阻止安装的依赖关系。通常情况下，修复对某一特定版本的依赖性或删除不兼容的代码就足够了。详细信息请使用命令：`composer why baraja-core/doctrine`。

更新项目和包
-----------------------------

一个设计良好的项目的开发，使你可以很容易地随着时间的推移下载更新，并始终拥有所有软件包的最新版本。主要的好处是，你可以得到所有的bug修复，通常是性能的改进和大量的新功能。此外，渐进式的转变将使长时间后的更新变得不那么复杂，因为你将在较小的范围内即时修复问题，并避免不兼容的情况。

要更新所有的软件包和依赖项，请使用`composer update`命令。

在某些情况下，更新过程本身可能会失败。原因通常是依赖关系破裂或软件包不兼容。

如果想了解软件包无法安装的详细信息，请使用命令：`composer why-not baraja-core/doctrine`。如果我们已经有了软件包，而只有特定的版本不工作（它不愿意安装），我们也可以要求特定的版本：`composer why-not baraja-core/doctrine:v1.0.20`。

在`composer.json`文件中，我们也可以列出对特定运行时的依赖性。当我们和多人一起开发一个项目，并想验证他们是否安装了所有的扩展时，这一点特别有用。

通常情况下，会检查PHP的版本（必须是`7.1.0`或更高版本）。

```json
{
   "require": {
      "php": ">=7.1.0"
   }
}
```

可能是其他系统扩展。

```json
{
   "require": {
      "php": ">=7.1.0",
      "ext-json": "*",
      "ext-session": "*",
      "ext-PDO": "*"
   }
}
```

然后在安装软件包或升级时考虑到这些规则。这有助于防止在运行时变得明显的问题。通常，例如，一个支付网关包需要与API通信，所以它必须承认对`curl`和`json`扩展的依赖。

排除依赖关系的故障
-----------------------------

通常情况下，依赖性违反的发生是因为PHP的版本不好。在这种情况下，Composer会抛出一个信息，例如`你的PHP版本（7.3.11）被 "config.platform.php "版本（7.1）覆盖，不满足该要求。

很多时候，错误是由直接在项目`composer.json`中的设置引起的，其中有以下部分。

```json
"config": {
   "platform": {
      "php": "7.2"
   }
}
```

更改**必须直接在文件中进行**。在全局项目的情况下（安装前或有全局依赖），你可以用`composer config -g platform.php 7.2.14`强制使用Composer版本（`-g`开关意味着`global`）。

在某些情况下，我们想安装最新的软件包版本，而忽略本地环境设置。在这种情况下，我们可以使用高级命令。

```shell
$ composer update --ignore-platform-reqs
```

**使用`ignore-platform-reqs`开关，风险自负，可能会对项目造成损害！**如果你不了解后果，请不要使用它。

手动合成器的调用、参数和内存管理
------------------------------------------------------

Composer实际上是一个包裹在所谓PHAR中的PHP脚本，PHAR是一个PHP应用程序的编译版本。知道这些信息可以得到很好的利用，例如可以更好地对调用本身进行参数化。

在真正的大项目中，有时会发生我们的内存用完了，需要分配更多的内存，或者增加脚本的处理时间。

例如，在Windows上，你可以使用这个命令来实现。

```shell
$ php -d memory_limit=-1 C:/xampp/htdocs/composer.phar update
```

`memory_limit=-1`开关告诉Composer不受内存限制的影响，需要多少内存就消耗多少。

在Composer动作之后的自定义用户脚本
--------------------------------------------

在运行Composer动作后，你可以调用自动执行用户定义的脚本，这些脚本要么在项目上执行特定的动作，要么在部署后生成一个配置。我主要是在本地服务器上使用这种方法，提供一个自动数据库配置工具，生成一个数据库模式等等。

我们在`composer.json`的`scripts`部分注册所需的脚本。

```json
"scripts": {
   "post-autoload-dump": "Baraja\\PackageManager\\PackageRegistrator::composerPostAutoloadDump"
}
```

在这种情况下，`Baraja/PackageManager/PackageRegistrator'类中的静态方法`composerPostAutoloadDump'被自动调用。Composer之所以有这个类，是因为它首先进行了<a href="/autoloading-trid">autoloader类</a>的生成。

如果我们只想运行脚本，而不想用Composer进行不必要的操作，`composer dump`命令就非常有用，因为它只是生成一个新的自动加载器（我建议始终保持更新），然后立即运行脚本。如果你想尝试使用脚本，我准备了一个现成的包[Baraja PackageManager](https://github.com/baraja-core/package-manager)，它实现了Nette框架的智能脚本和互动接口。

将供应商的版本信息输入Git？
------------------------

我们经常和开发者讨论的一个问题是，是将`vendor`文件夹的内容版本化到Git中，还是在每次安装时重新生成。

一般来说，更清洁的解决方案似乎是**根本没有版本**内容，每次都安装所有内容。但现实是，不时有开发者会停止开发一个软件包，或完全删除它。不断地下载软件包也使本地安装和更新变得复杂，同时也拖慢了部署速度，当新的软件包版本被错误地下载时，有时会造成网站的短暂中断。

我认为Vendor的停职是一种 "安全 "的形式。如果这些文件都在版本系统中，我们在生产服务器上至少有一个基本的保证，即这些包可以工作，它们的代码与本地运行的安装中的完全一样。此外，Vendor往往只占用单位MB，考虑到今天的磁盘容量，这当然值得保证一个工作站点。

**实践说明：**对于我所维护的普通网站，`vendor'占用的空间不超过`30MB'，这对Git来说是可以接受的容量。当与小辈克隆一个资源库时，我们就不必再去培训他如何让网站运行，它只是 "马上就能运行"。

自定义作曲家包
-----------------------

你可以在Composer中创建你自己的包，可以是公开的（在[Packagist](https://packagist.org/)注册），也可以是私有的（你必须有自己的包服务器，例如[Satis](https://getcomposer.org/doc/articles/handling-private-packages.md)）。

创建、维护、开发和版本管理包的问题非常复杂，将是另外一篇文章的主题。

同时，你可以阅读我为你翻译的[Semantic Versioning]（https://semver.org/lang/cs/）这篇文章。
