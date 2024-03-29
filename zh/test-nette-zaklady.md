对Nette基本知识的测试
=============

> id: d137c177-503c-4e84-8709-0e65b0ce6060
> slug:
> 	cs: test-nette-zaklady
> 	zh: duinette-ji-ben-zhi-shi-de-ce-shi
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '66fb122b33aa8ce41158345bb01769ab'

成功门槛：15分

*每答对一个问题，你就得到1分。对于任何回答错误的问题，你什么都得不到。如果答案只是部分的（而且不可能根据它来编程），那么这个问题就算是不正确的（不可能得到半分）。 如果解决方案包含一个安全漏洞，或者代码中有一个错字，或者代码中有一个错字，那么这个答案就算是不正确的，因为它不会运行。

-----------

1 解释一下 "for"、"while "和 "foreach "循环的区别。对于每一项，请举出一个具体的例子，清楚地表明其主要优势。


2.我们有一个变量，我们对它几乎一无所知（我们只知道它的名字）。我们怎么能看到它的内容呢？例如，它被称为"$data"。


3.编写下面的命令来处理 Git 仓库。
- 从服务器上下载最新的变化
- 给 "Statistic.php "文件加上标签
- 标记项目中的所有文件
- 标记`cron`目录下的所有文件
- 提交修改的信息 "我的提交"
- 向服务器发送提交信息


4.让我们在变量里有一个文本字符串。举一个计算校验和的函数的例子。


5.编写一个代码片段，在 "Presenter "中创建一个 "delete "动作，接受作为整数的项目ID，并根据指定的ID从 "question "表中删除一条记录。删除成功后，它将打印出 "问题已删除 "的信息，并重定向到 "列表 "动作。

在问题下获得加分。如果由于某种原因删除失败，它不会抛出一个致命的错误，但也会用一条信息（flash message）告知用户。

6.当我创建一个Nette表单时，它成为一个组件。什么是Nette组件？

7.我需要创建一个简单的Nette表格，将一条记录插入一个包含问题列表的`question`表。该表的结构是。

| 专栏 | 属性 |
|-----------|----------------------------------|
| id | int(8), 无符号, 自动递增 |
| 问题 | varchar(255) |
| is_active | tinyint(1), 无符号, 默认值: 1 |

创建适当的表格字段，向该表插入新行。在插入记录后，必须发射一个FlashMessage，告知成功插入记录+重定向到编辑记录（`edit`动作）。

- 验证表单字段已被填写
- 验证问题文本是否符合表结构的varchar格式
- 验证该文本的问题不再存在
- 定义另一个自定义的 "组 "表，以包含关于组的信息。当创建一个问题时，就可以确定该问题属于哪个组。你需要在各表之间建立一个会话（描述如何做到这一点，以及将如何设置）。
- 我应该用什么Latte宏来渲染表单到模板（默认渲染）？

8.让我们在`Presenter`里有一个编辑表单，它是作为一个组件创建的。我们想从数据库中传入默认值，也就是说，我们需要以某种方便的方式从表中获取数据。
- 你将如何进行，我们需要代码中的哪些元素？
- 你将如何把当前编辑的项目的标识符传递给表单？
- 如何设置表单元素的默认值？
- 我们如何验证一个用户正试图编辑一个在数据库中不存在的项目，以及我们如何适当地通知他们这一点？

9 考虑从一个数据库中检索出以下数据（使用普通的Nette数据库）。

```php
$questions = $this->db->questions()->fetchAll();
```

- 我们如何将所有问题的文本列成一个圆点状的清单？
- 我们如何将数据从表中传递到Latte模板？
- 我们需要什么拿铁宏来列举这些项目？给出一个具体的实现，在格式中列出`id`和`name`列。

	*1024：你好吗？
	*1025：你今天午餐吃了什么？

10.列举一个至少有3个不同表单字段的例子，写在表格中。

```php
$form->add(tady bude příklad);
```

并为每一个解释它的用途和它返回的输出（数据类型+例子）。


11.让我们有一个提交的Nette表格。
- 我们如何获得所有字段（名称和值）的发送？
- 举个例子，输出一个名为 "question "的字段。
- 提供一个具体的实现代码，该代码将遍历值和键的数组，并返回一个包含键和值的总列表的单一字符串，这样我们就可以，例如，将这个字符串存储在数据库中或通过电子邮件发送（存储和发送不是任务的主题，不会被评估）。


12.对于每个条件，决定结果是 "真 "还是 "假"。
- `1 > 0`
- `1 == 1`
- `1 == "1"`
- `1 === "1"`
- `1 == true`。
- `1 === true`。
- `1 === false`。
- `1 == "1" && 1=== true`。


13.我们在PHP中熟悉哪些数据类型？
- 至少给出5个数据类型名称的例子
- 对于每个例子，至少要列出3个可以存储在数据类型中的可能值（如果数据类型不能存储这么多可能值，就写上）。
- 对于每一种数据类型，请举出其使用的典型例子（在实践中储存什么）。
- `==="（两个等于）和`==="（三个等于）的条件有什么不同？
- 解释在条件中使用"==="的缺点，以及"==="是如何具体解决这个问题的（举例说明"==="可能失败，而"==="可以挽救这个局面）。


14.让我们有一个协调表（coordinations table），列出2个人之间的所有协调。他们中的一个人组织协调，另一个人是客人。编写一个数据库选择，返回所有涉及我的协调的行（我是协调的组织者，或者我是协调的客人）。该表有列`id`，`id_user_organizer`（组织者ID），`id_user_quest`（客人ID）。我的ID是以通常的方式存储在`Presenter`中。


15.关于Latte的一组问题。
- 什么是拿铁？
- 变量"、"宏"、"过滤器 "和 "n:属性 "之间有什么区别？什么东西用在哪里？
- 如何为 "默认 "动作创建一个 "DashboardPresenter "引用？
- 列出一个问题表，如何生成一个链接到一个问题的特定编辑（`QuestionPresenter`，`edit`动作），以传递当前列出问题的ID？编写具体的拉蒂代码。

符号化的写法（PHP中的样本，翻译成Latte）。

```php
foreach ($questions as $question) {
   echo $question->id; // 问题编号
   echo $question->question; // 问题文本
}
```

- 如何写一个固定的HTML空间？


16.Nette中的服务是用来做什么的？
- 它是如何被实例化的？
- 一项服务必须做什么才能被使用？
- 我如何将一个服务加载到Presenter中？
- 例如，考虑 "StatisticManager "服务，它有一个公共方法 "getStatistics()"，不接受参数。我如何在Presenter中加载这个服务并在默认动作中调用公共方法`getStatistics()`，并将结果传递给模板？
- 对象"、"类"、"服务 "之间的区别是什么？
- 什么是 "模型"、"实体 "和 "价值对象"？
- 所有的服务都有一个认识它们的主管理器，并可以在该管理器中被拾取。这位经理的名字是什么？我如何在其中注册一个新的服务？


17.霓虹灯
- 什么是霓虹灯文件？
- 有哪些类型，它们是按什么分类的？
- 霓虹灯文件包含什么？其中存储了哪些数据？
- 请举例说明如何注册以下字段（配方是PHP的，需要翻译），以便可以作为参数使用。

```php
$imageGenerator = [
   "点" => [
      480: [910, 30, 1845, 1150],
      600: [875, 95, 1710, 910],
      768: [975, 130, 1743, 660]
   ]
];
```

- 举个例子，在Neon中注册一个服务，我们把在上一个任务中注册的`imageGenerator'参数传给它，这样服务在构造函数中就会收到它，并能在服务中使用它（在配置意义上）。对于服务，提供一个构造函数的示例实现，使第一个输入参数被视为数组的数据类型。


18. 一般的物体
- 什么是 "方法"、"属性 "和 "常量"？它们之间有什么区别？
- 方法和属性都有3种基本的可访问性状态（"公开"、"私有"、"保护"），解释两者的区别和一个具体的使用例子，以及谁可以看到什么，什么时候。
- 我有一个`课程'类，其中有一个私有属性`currentCourse'，当前课程被存储在其中。如何使该属性只读而不从外部写入？


19.当我在数据库中创建逻辑上相关的表时（例如，一个用户的表，然后是他的文章的表），我需要处理数据将被正确链接。
- 什么能确保表内的数据在数据库中被正确链接？
- 什么是参考完整性，它在数据库中的作用是什么？
- 我们有哪些类型的会议？每种类型的目的是什么？
- 我们为会话设置什么参数，以不同方式处理数据的删除或修改？举出3个例子和具体用途+描述它的工作原理。


20.工厂（"OOP设计模式"）的目的是什么？
- 举一个使用的例子。
- Nette服务是工厂吗？
- 依赖性注入的目的是什么？
- DI "和 "DIC "之间的区别是什么？
