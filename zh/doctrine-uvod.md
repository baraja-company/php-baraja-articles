教义系列 - 简介
=========

> id: fbd0461a-53fe-4713-8926-82e31bd1fb9b
> slug:
> 	cs: doctrine-uvod
> 	zh: jiao-yi-xi-lie-jian-jie
> 
> publicationDate: '2021-08-27 11:40:00'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: '2bc62308fcb66acda5a09ac95b5eb2c2'

Doctrine是一个先进的PHP库，用于面向对象的数据库工作。Doctrine的主要目的和目标是使用数据实体描述数据库模式，并以完全面向对象的方式操作数据。

这种范式被称为ORM（Object-relational mapping），它是[设计模式]（/design-pattern），用于将存储在关系数据库中的数据转换（包装）为可用于面向对象语言的对象。因此，要理解和使用Doctrine，你至少要知道[面向对象编程](/oop)的基础知识。

为什么要学习教义？
------------------------

有很多原因。

- Doctrine是使用最广泛的ORM数据库，被大多数高级PHP社区所使用。
- 它将极大地简化你的PHP应用程序的设计
- 你提供了一个一致的方式来设计、版本、传输和备份你的数据库模式
- 你可以[通过下载软件包获得大量的数据库表](https://github.com/baraja-core/shop-product)，而不需要搞清楚和配置任何东西。
- 表之间的关系成为真正的物理实体
- 数据库的输出不会是普通的无类型的数组，但你会得到真正的物理对象
- 你可以得到一个简单的方法，在一个单一的交易中同时执行许多操作
- 你只需知道什么时候发生，而且是安全地发生，就可以轻松地提高应用程序的安全性和复原力。
- 你会得到一个容易测试的代码和数据库层
- 你会发现围绕Doctrine的整个生态系统能够优雅地解决许多问题。你经常会发现一些复杂问题的简单解决方案，否则几乎不可能轻易解决。
- 你会学到很多新的东西，探索新的想法，并充分使用数据库的潜力。
- 摆脱了复杂的SQL查询。Doctrine提供了一个非常强大的自定义查询编写界面（DQL）。
- 应用会越来越快。你将很容易发现应用优化的空间，利用懒惰加载的优势，并找到应用瓶颈。

本文作者（[Jan Barasek](https://baraja.cz)）长期坚持的观点是，Doctrine是与PHP数据库合作的最佳方式。它根本没有竞争对手。

如何开始？
----------

在你开始全面使用Doctrine之前，你需要准备一个合适的环境。如果你刚开始接触PHP或者没有高级知识，最好的选择是用[Baraja Doctrine](https://github.com/baraja-core/doctrine)扩展包安装Nette框架，它自动集成了全面支持。首先通过[Composer](/composer)下载软件包，然后设置DI扩展，Doctrine将自动开始工作。

为了让Doctrine正常工作，你需要准备一个空的数据库（Doctrine也可以在现有的项目中工作，但这对第一步来说是不合适的，因为它有覆盖现有数据的风险）并配置连接。由于Doctrine不仅仅是一个数据库库，而是提供了一个先进的数据库框架，你需要[解决其他配置](/configure-connections-with-baraja-doctrine)。大多数设置在该软件包中被自动覆盖，然而在最低配置中，你的服务器必须支持`APCu Cache`或`SQLite3`扩展。

如果一切配置正确，一个新的DI服务`Baraja\Doctrine\EntityManager`将在Nette中被创建，你可以把它[注入](https://doc.nette.org/cs/3.1/di-usage)到Presenter。

```php
namespace App\FrontModule\Presenters;

use Baraja\Doctrine\EntityManager;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

如果你设法注入基本的EntityManager服务，你就可以开始学习和使用Doctrine。

如何进行？
--------

以下各章结合了Doctrine技术参考指南、多年的经验、设计模式和现成的解决方案。我们将一起探讨Doctrine的所有基本要素，从定义你自己的实体，到生成物理数据库模式，再到使用版本工具和生产部署。

我使用Doctrine已经很长时间了，并在其中解决了成千上万的案件。我们将展示如何使用Doctrine来优化数据库速度以及如何合理设计数据库的技巧和窍门。你也可以对现有项目使用Doctrine（如果你满足某些条件），我们将向你展示如何做到这一点。

这一系列的文章是为了帮助我的培训和咨询学生。如果你需要更详细地讨论或解释某些主题，你可以给我发电子邮件，jan@barasek.com。由于这是一项要求相对较高的技术，所有的问题都将被视为付费咨询。
