明年人工智能抢走了程序员的工作，该怎么办？
=====================

> id: '0eccb5c1-9ab4-42bf-b27e-13917d616b7f'
> slug:
> 	- null
> 	zh: ming-nian-ren-gong-zhi-neng-qiang-zou-le-cheng-xu-yuan-de-gong-zuo-gai-zen-me-ban
> 
> cs: openai-php-kod
> publicationDate: '2022-12-23 13:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '573e4173dd9e7a2e7068c404fecf9b72'

OpeanAI语言模型的可能性是不可思议的。只要输入源代码，要求对其进行分析、修正并改写成另一种语言，一切都会自动发生。

当我第一次尝试时，我对可以实现的目标感到震惊，以及普通大众可以使用的工具是多么强大。

我们来看看一个例子。

**问题：**找出该函数的安全漏洞，并提供修补后的代码。

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'一月',
		'2月',
		'三月',
		'四月',
		'5月',
		'6月',
		'七月',
		'八月',
		'9月',
		'10月',
		'11月',
		'12月',
	];

	return $czMonths[(int) date('m')];
}
```

**答案：**

这一功能的安全漏洞之一是利用SQL注入攻击的危险。当格式不良或恶意的输入被传递给函数时，数据库可能被破坏或利用。

一个修复方法是使用预定义的月份常数，而不是通过调用date()函数获得的输入。我们也可以使用filter_var()函数，加上一个适当的过滤器，以确保输入的是数字，并且不包含恶意代码。

以下是更正后的代码。

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'一月',
		'2月',
		'三月',
		'四月',
		'5月',
		'6月',
		'七月',
		'八月',
		'9月',
		'10月',
		'11月',
		'12月',
	];
	$month = filter_var((int) date('m'), FILTER_VALIDATE_INT);

	if ($month === false || $month < 1 || $month > 12) {
		return "";
	}

	return $czMonths[$month];
}
```

**问题：**将函数的实现转换为typecript。

**答案：**

下面是该函数在TypeScript中的重写实现。

```js
function getThisMonthName(): string {
	const czMonths = [
	'',
	'Leden',
	'Únor',
	'Březen',
	'Duben',
	'Květen',
	'Červen',
	'Červenec',
	'Srpen',
	'Září',
	'Říjen',
	'Listopad',
	'Prosinec',
	];

	const month = filter_var((int) date('m'), FILTER_VALIDATE_INT);

	if (month === false || month < 1 || month > 12) {
		return "";
	}

	return czMonths[month];
}
```

**请求：**这不是一个有效的手稿!

**答案：**

我对这个错误表示歉意。下面是TypeScript中该函数的修正版本。

```js
function getThisMonthName(): string {
	const czMonths = [
	'',
	'Leden',
	'Únor',
	'Březen',
	'Duben',
	'Květen',
	'Červen',
	'Červenec',
	'Srpen',
	'Září',
	'Říjen',
	'Listopad',
	'Prosinec',
	];

	const month = Number(new Date().getMonth()) + 1;

	if (month < 1 || month > 12) {
		return "";
	}

	return czMonths[month];
}
```

注意：TypeScript中没有filter_var()函数，所以我通过调用Date类中的getMonth()函数来替代它，然后用Number()函数将其转换为数字。这个版本的函数在TypeScript中应该是有效的。

**问题：**你对Typescript的实现是否包含安全漏洞？

**答案：**

我在TypeScript中对该函数的修正实现应该不包含原来PHP实现中存在的SQL注入攻击的安全漏洞。 Date类的getMonth()函数返回一个数字的月份值（0代表1月，1代表2月，等等），所以我们不需要从date()函数中传递输入，可以避免SQL注入攻击漏洞的危险。

然而，任何应用程序都可能包含安全漏洞，因此，定期检查代码以寻找潜在的安全问题是很重要的。如果你怀疑你的代码中存在安全漏洞，你可以联系安全专家或使用自动安全漏洞扫描工具。
