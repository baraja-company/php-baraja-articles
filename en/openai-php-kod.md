What to do when artificial intelligence takes jobs away from programmers next year?
===================================================================================

> id: '0eccb5c1-9ab4-42bf-b27e-13917d616b7f'
> slug:
> 	- null
> 	en: what-to-do-when-artificial-intelligence-takes-jobs-away-from-programmers-next-year
> 
> cs: openai-php-kod
> publicationDate: '2022-12-23 13:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '573e4173dd9e7a2e7068c404fecf9b72'

The possibilities of the OpeanAI language model are incredible. Just input the source code, ask for its analysis, correction and rewriting into another language, and everything happens automatically.

When I first tried it, I was shocked at what can be achieved, and how powerful the tools are available to the general public.

Let's look at an example:

**Question:** Find the security vulnerability in this function, and provide the patched code:

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'January',
		'February',
		'March',
		'April',
		'May',
		'June',
		'July',
		'August',
		'September',
		'October',
		'November',
		'December',
	];

	return $czMonths[(int) date('m')];
}
```

**Answer:**

One of the security vulnerabilities in this feature is the danger of exploiting SQL injection attack. When poorly formatted or malicious input is passed to the function, the database can be compromised or exploited.

One fix is to use predefined month constants instead of input obtained from a call to the date() function. We can also use the filter_var() function with an appropriate filter to ensure that the input is numeric and does not contain malicious code.

Here is the corrected code:

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'January',
		'February',
		'March',
		'April',
		'May',
		'June',
		'July',
		'August',
		'September',
		'October',
		'November',
		'December',
	];
	$month = filter_var((int) date('m'), FILTER_VALIDATE_INT);

	if ($month === false || $month < 1 || $month > 12) {
		return "";
	}

	return $czMonths[$month];
}
```

**Question:** Convert the function implementation to typescript.

**Answer:**

Here is the rewritten implementation of the function in TypeScript:

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

**Request:** This is not a valid typescript!

**Answer:**

I apologize for the error. Here is the corrected version of the function in TypeScript:

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

Note: There is no filter_var() function in TypeScript, so I replaced it by calling getMonth() from Date class and then converting it to a number using Number(). This version of the function should be valid in TypeScript.

**Question:** Does your implementation of Typescript contain a security vulnerability?

**Answer:**

My corrected implementation of the function in TypeScript should not contain the security vulnerability of SQL injection attack exploit that was originally present in the original PHP implementation. The getMonth() function of the Date class returns a numeric value of the month (0 for January, 1 for February, etc.), so we don't need to pass input from the date() function and can avoid the danger of SQL injection attack exploit.

However, any application can contain security vulnerabilities, so it is important to periodically examine the code to look for potential security issues. If you suspect a security vulnerability in your code, you can contact security experts or use automated security vulnerability scanning tools.
