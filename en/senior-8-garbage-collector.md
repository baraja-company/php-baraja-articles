Inappropriate use of the Garbage Collector
==========================================

> id: '842e30ab-1af1-4152-ba0f-3004ca3b620f'
> slug:
> 	cs: senior-8-garbage-collector
> 	en: inappropriate-use-of-the-garbage-collector
> 
> publicationDate: '2023-02-11 14:35:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: ae5993313436e0d7c5291b6337b988ef

You are a developer of a large legacy application, into which you are gradually introducing PHPStan. You start with level 0, which is quite challenging, but eventually you get it right. You move on to the next levels, where one part of your code starts reporting an unused $lock variable that you should remove.

The code looks like this:

```php
public function processOrder(int $orderId): void
{
	$lock = Lock::createLock('order-' . $orderId);

	// There's some logic here...
}
```

You tell yourself that there must be a lock stored in the variable that someone forgot to release later, or maybe it's happening inside other methods that are called later. So you decide to remove the unused variable, keeping only the call to the static method that creates the lock.

Can this decision cause a critical error?

If so, why, and how might the original mechanism have worked?

If not, why not, and how do you know that this is always a safe operation?
