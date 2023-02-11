不适当地使用垃圾收集器
===========

> id: '842e30ab-1af1-4152-ba0f-3004ca3b620f'
> slug:
> 	cs: senior-8-garbage-collector
> 	zh: bu-shi-dang-de-shi-yong-la-ji-shou-ji-qi
> 
> publicationDate: '2023-02-11 14:35:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: ae5993313436e0d7c5291b6337b988ef

你是一个大型遗留应用程序的开发者，你正在逐步将PHPStan引入其中。你从0级开始，这相当具有挑战性，但最终你会得到它。你进入下一个层次，你的代码的一部分开始报告一个未使用的$lock变量，你应该删除它。

代码看起来像这样。

```php
public function processOrder(int $orderId): void
{
	$lock = Lock::createLock('订单-' . $orderId);

	// 这里有一些逻辑...
}
```

你告诉自己，一定有一个锁存储在变量中，后来有人忘了释放，或者也许它发生在后来被调用的其他方法中。所以你决定删除这个未使用的变量，只保留对创建锁的静态方法的调用。

这个决定会不会造成关键的错误？

如果是这样，为什么，原来的机制可能是如何运作的？

如果没有，为什么没有，你怎么知道这始终是一个安全的操作？
