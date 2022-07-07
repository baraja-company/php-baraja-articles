在PHP中进行解析和数据处理
==============

> id: '7dfc991d-a7ee-4e2b-8159-16cef1d27483'
> slug:
> 	cs: parsovani-a-zpracovani-dat
> 	zh: zaiphp-zhong-jin-xing-jie-xi-he-shu-ju-chu-li
> 
> publicationDate: '2017-10-15 09:54:02'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '33947488a0005f2977e6283c532a08d7'

> 本文将讨论在PHP中处理数据的方法，但还没有结束。

因此，暂时只是对各种可能性做了一个简单的概述。

- **逐个字符抓取**是一个非常古老的方法，它使代码变得混乱，但所有其他方法都在内部进行。
- <a href="/explode">Explode</a>，通过分隔符来分割一个字符串
- <a href="/regex">**常规表达式**</a>是处理简单字符串的最佳方式。
- **Tokenizer**，根据正则表达式将复杂的字符串分割成几块（tokens），例如，PHP是这样处理的
