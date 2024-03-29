从UUID返回到整数
==========

> id: d842517b-c4f6-4cef-a839-d08ff3804fca
> slug:
> 	cs: navrat-z-uuid-na-integer
> 	zh: conguuid-fan-hui-dao-zheng-shu
> 
> publicationDate: '2021-05-02 17:00:00'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: b7c136f3e3ca8a4563230c557ab1fa9c

在软件开发中，当程序员面临一个对他或她未来几十年的工作有巨大影响的架构决定时，往往会走入死胡同。同时，这也是一个无法逆转的决定，每一个错误都要付出惨痛的代价。数据库是架构决策的一个典型例子，每一个小错误都会让人头疼。

近来的一个重大决定是如何在数据库表中存储主键。虽然这似乎是一个微不足道的问题，但其背后有很多东西比你想象的要多。

主键选项
-------------------------

基本上有四个基本选项。

- 整数
- 整数 无符号
- 大英数
- <a href="/uuid-performance">UID</a>

整数是一个简单的整数（如果是 "无符号 "则是无符号，所以总是正数，如果是 "大尺寸 "则可以达到非常大的数值）。一个非常简单的概念。然后，UUID是一个文本字符串（例如，形式为`c4a760a8-dbcf-5254-a0d9-6a4474bd1b62`），由几个部分组成，每个部分都可以有某些属性，对建立巨大的多服务器或分散的应用程序很有用。围绕UUID有一个庞大的有用技术生态系统，解决你可能不知道的问题，或在未来会有的问题。

使用正确的锤子
-------------------------

不久前（2020年冬天），我的朋友保罗正在解释将适当的解决方案应用于一个特定规模的问题的概念。这是一个伟大而重要的想法，但许多开发人员喜欢忘记--它在不需要的时候创造了巨大的复杂解决方案。在英语中，我们有一个很好的短语来形容这种**过度的工程**。

UUID的大小和唯一性
--------------------------

UUID的根本优势在于，如果应用程序变得太大，我们将数据库分割到许多网络服务器上，其中一个数据库表非常巨大，以至于无法装入一台机器的磁盘，我们可以将其分割到许多物理机器上，每台机器都会知道自己的那块表，并查询其同事的其余部分。UUID还解决了插入新行的基本问题，当在极其庞大的应用程序中，我们需要在许多地方并行地写入行，而且我们不想等待主服务器的写入能力释放出来。

同时在许多地方写作的概念，例如被聊天应用程序使用。当你通过Messenger发送消息时，消息会被发送到最近的Facebook数据库服务器，该服务器会给消息分配一个UUID和时间戳，并将其写入其本地数据库中。你在世界另一端的朋友，反过来把信息写到他的本地数据中心，与此同时，整个云基础设施确保了全球范围内的同步。听起来很酷，对吗？:)

为了使这种平行写作发挥作用，需要解决记录碰撞的问题。如果各个本地数据库使用一个简单的整数，很快两个独立的服务器就会在同一个标识符下写入两条不同的记录。当这些记录被同步时，就会发生碰撞。通常没有解决办法--你不能给ID重新编号，因为其他会话可能会导致这种情况。

UUID解决了这个问题，例如，给每个服务器一个商定的前缀，它插入每个UUID的开头，然后插入一个时间戳，然后是标识符本身。

> **有趣的事实：**在写入如此大量的数据时，我们对什么时候写入什么记录并不感兴趣，而是对写入的顺序感兴趣（比如说，这样我们就不会为用户调换消息的顺序）。

你可能会问，如何处理服务器之间不能就使用哪个前缀达成一致的情况。例如，这个问题出现在分散的或离线的应用程序中。在这种情况下，UUID甚至可以随机生成。

那么问题来了，在<a href="https://stackoverflow.com/questions/1155008/how-unique-is-uuid">随机生成UUID</a>时，发生冲突的几率有多大。那么，它可能不会发生在你身上。大约有`2^122`个唯一的UUID（因为它是一个`128位数字`）。在实践中，发生冲突的机会大约是`0.00000000006（6×10-11）`。在实践中，这意味着如果我们在未来100年内每秒产生**10亿个UUID**，发生冲突的机会将是`50%`。所以冲突更不可能发生，UUID是解决你的数据库问题的最终方案。

是否需要这样一个强大的解决方案？
-------------------------------

如果你不知道，答案是**不**。

当主键设置为 "int "并带有 "unsigned "标志时，有 "4,294,967,295 "个可能的值（40亿）。关于整数大小的比较，见<a href="https://dev.mysql.com/doc/refman/8.0/en/integer-types.html">MySql文档</a>。

当你在一个表中存储40亿条记录时，你可能会更快地耗尽磁盘空间。

整数和连接性能
----------------------

整数真的非常快。在MySql中对它们有本地优化。索引可以正常工作（另外它们的体积要小得多），它们只需要4个字节，它们的连接速度非常快，对于大多数情况来说，它们会很好。

如果你正在处理数据库复制问题，最好的解决办法可能是把整个数据库放在MS Azure这样的云中，并从外部查询。即使在存储数千万条记录时，通过整数访问特定行的速度也在毫秒级（在配置良好的服务器上低于3毫秒），而且有了聚类索引，即使有大量的请求，时间也可以很好地持续。

如果你真的需要使用UUID，你最好离开MySql世界，走Postgres数据库路线，与MySql不同，它有自己的数据类型用于<a href="https://www.postgresql.org/docs/9.1/datatype-uuid.html">UUID</a>。在UUID和MySql中，连接工作是一个巨大的问题，当只连接3个表时（每个表只有几万条记录），整个查询可能需要几百毫秒到几秒的时间来处理。而这不幸是一个MySql问题，你可能无法解决。
