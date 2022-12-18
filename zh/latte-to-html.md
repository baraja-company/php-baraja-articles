如何将Latte模板渲染成一个字符串
==================

> id: '847b028d-c4e6-4480-afe4-797cf83fce81'
> slug:
> 	cs: latte-to-html
> 	zh: ru-he-jianglatte-mo-ban-xuan-ran-cheng-yi-ge-zi-fu-chuan
> 
> publicationDate: '2022-12-18 11:30:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '0647f751eb754e9cf4afa9f3d0b45e17'

Latte模板系统适用于在网络上呈现几乎所有类型的模板。例如，对于渲染前端模板，React或Vue.js在过去几年中一直是最好的选择，但对于在后端渲染电子邮件模板，Latte仍然获胜。

那么，我们如何确保将一个特定的HTML模板渲染成一个可以通过电子邮件发送的字符串？

容易。

```php
$latte = new Engine();
$latte->setLoader(new StringLoader());

$template = '<p>我的名字是。{$firstName}：{$lastName}！</p>';

$html = $latte->renderToString($template, [
	'第一个名字' => '扬',
	'最后的名字' => '测试',
]);

echo $html;
```
