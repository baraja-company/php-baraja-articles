一个程序员如何在内部开发团队中生活
=================

> id: '9309b0f2-0265-43aa-a9c3-10b2d486cdfc'
> slug:
> 	cs: jak-zije-programator-v-internim-tymu
> 	zh: yi-ge-cheng-xu-yuan-ru-he-zai-nei-bu-kai-fa-tuan-dui-zhong-sheng-huo
> 
> publicationDate: '2022-01-10 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: e65dd78fa9813b552f30b98f04aacb30

诚实是要付出沉重代价的。

这个网站一直在描述从事IT行业的人所经历的现实，所以我想看看我在开发团队工作的经历。以下是我在各公司的一般经验。没有任何经验与某个特定的公司有关，不一定作为批评。

公司往往不需要努力工作和积极主动的人
----------------------------------------------

有很多想法吗？你想创新吗？你是否喜欢为你的团队正在解决的、困扰半个公司的复杂问题找到优雅的解决方案？你是否意识到安全、软件设计和寻找项目瓶颈的重要性？

你可能会在开发团队中不开心很长一段时间。

团队合作是我最近一直在挣扎的事情--我的意思是，具体来说，我一直在其中游泳，并试图理解那些对我来说完全不直观的原则，以便坚持下去。

- 团队合作意味着制定整个团队都能理解的解决方案。这往往不是最好的。它几乎从来都不优雅。事实上，它始终是低效的。但它有一个很酷的优势，那就是整个团队都能理解它，并且可以长期管理它。这是一个极其重要的想法。因为有了大项目，就没有那么大的压力，因为作为一个公司，你可以通过人去扩展，你有足够的资金。按时交付东西要重要得多，即使部分破损，事先有未知的限制，但也要按时交付。这与这样的想法有关：你明天交付的解决方案（即使是不完美的）对客户来说比100%调试的解决方案在一年内不会出现的价值要大得多。
- 创新和推陈出新是相当错误的。这与以下事实有关，即在公司工作的人有家庭，只想在工作时间工作。这必然导致人们学习和尝试新事物的时间有限。从本质上讲，公司必须在人们的工作时间内塞进可用于未来工作的教育和发展。那么，一个有趣的后果是将决定和学习新事物的时间推迟到必要的时候。就我而言，我理解这种做法，它对我来说有很大的意义。如果我有员工，我也会更喜欢平静的人，他们会在公司持续工作，比如说15年，而不是那些有很好的总体概述并想继续前进的人，但这将是以牺牲团队为代价。诚然，集体的解决方案永远不会与个人的解决方案相同，但这并不意味着它将会更糟。

像我这样的人是问题和冲突的潜在来源。这样想就很酷。

当你进行代码审查时，只需寻找客观错误即可
----------------------------------------

每个公司都有自己的代码风格规则。不幸的是。起初，它让我非常恼火。

当你使用更成熟的语言之一，如.NET，代码风格规则往往更相似。这时你才发现，PHP和JavaScript仍然存在于这个世界上。尤其是PHP的事情，有时让人有点沮丧。PHP是一种非常成熟的语言，有许多伟大的功能，但根据我的经验，它在公司中的使用量大约只有其总潜力的三分之一。原因往往不同，最常见的是惯性。

在这方面，我长期以来一直在寻找代码审查的程序性解决方案。我已经玩了很长时间的PhpStan，并关注着它周围的想法。PhpStan的基本理念是，它只强调在某些时候肯定会发生的客观错误。我越是探索PhpStan并将其提升到新的水平，我就越是在内心验证它是多么真实。

如果PhpStan能在至少一半的捷克公司中实施，那就太好了。如果至少达到6级就更好了。 在实践中，我看到最好的公司有4级或5级。

就在前几天，我还在想，能不能有一个真正运行的生产应用，它有自己的开发团队，而且它甚至没有通过0级--那是控制任何应用中你所期望的东西的级别。类似于确保所有的类和函数都存在，文件没有解析错误，等等。是的，有这样的应用。但从长远来看，情况正在改善。但愿如此。

因此，我对codereview采取了类似的做法。我只报告那些客观上总是错误的事情。我经常遇到对程度的误判--有些事情是不对的，但同样，这并不是一个需要解决的大问题。许多问题是通过惯例解决的，或者宣布风险很小，所以没有必要治疗。

我一直认为缺少数据类型（甚至是复合数据类型）是一个重要的错误。然后我明白了，有测试和转储。

我不认为我对这部分有具体的结论。我只是对它有很多主观的评论，这些评论更有可能成为相互误解的来源，所以我不会张贴。长期来看，我倾向于这样的结论：我不能做codeeview--要么我太严格，要么太仁慈。我无法在一般的层面上分辨什么是真正重要的，什么是不那么重要的。我很想知道其他人是怎么做的。还没有人能够给我一个答案，我也可以用。

在定制开发中没有钱。
---------------------------------

你有一个机构并做定制的网络开发吗？如果你不够大，为其他公司做大项目，你可能有一天会不存在。

出现这种情况的原因是定制项目的资金不足。这可能与合同的性质有关，这些合同对买方来说更有利可图。事实上，大多数机构都是残酷的资金不足，只为业主赚取实际利润。

当你作为公司内部为自己开发一个项目时，延迟一个月的交付可能并不那么重要。作为一个机构，如果你没有在一个月前交付工作，你保证在所述的那个月在工作中缓慢入睡，持续破坏你的健康，客户对着电话和票据发誓，总是问是否可以更快，作为奖励，你仍然会支付合同的罚款。如果你不这样做，客户会给你贴上不可靠的承包商的标签，并可能考虑去其他地方，反正那里也不会有什么好结果。

但这些都是游戏规则，我在实践中已经知道了，这让我的预算出现了漏洞。

承包可能是IT领域最复杂的业务形式。你不希望做承包。签约将摧毁你。他们会在周末、晚上、圣诞节时给你打电话。你所做的一半工作，你没有得到报酬。你会为你不能犯的错误道歉。你会在Instagram上看那些今年第三次去度假的朋友的故事，而你却在周六凌晨3点坐在办公室里完成一个客户项目，反正周一你会被骂，因为合同里的内容比你能完成的要多。人们会请假期和病假，而你会在他们的位置上工作。否则你就会被解雇。然后你就会很高兴地支付租金。

但话说回来，你会学到很多东西。因为只要处于压力之下，就会给你带来很大的压力，让你去学习和提高效率，以便下一次你能在同样的时间内完成更多一点。糟糕的是，你无法将这些知识和经验真正应用到其他地方，因为公司根本不感兴趣（我在上面解释过）。

幸运的是，这是一个2019年的故事，而且距离现在还很远。

永远不会有一个理想的世界
-------------------------

如果我在做我喜欢的事情？你有吗？:D

我想这都是一个比例问题。你可能永远不会做一份100%满足的工作。你可能总是会有人向你解释，你不能做你已经强行学习了10年的事情，而你内心知道你可以。

另一方面，对自己的个性进行一定程度的否定，你就会获得拥有更多东西的空间。比如周末时间，更好的健康，更多的时间给自己，稳定的环境，等等。从长远来看，这也是显而易见的，它不可能持续下去。

我喜欢能够尝试不同的东西，亲身体验别人的情况。与定制工作相比，在开发团队中工作是一件非常无聊的事情。

这不再是寻找最佳和最快的解决方案，而是合作的问题。这是关于改善沟通、情感，以及最重要的是，学会做人。而这也是一个巨大的价值。
