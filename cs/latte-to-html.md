Jak renderovat Latte šablonu do řetězce
=======================================

> id: "847b028d-c4e6-4480-afe4-797cf83fce81"
> slug:
> 	cs: latte-to-html
>
> publicationDate: "2022-12-18 11:30:00"
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec

Šablonovací systém Latte se hodí pro renderování skoro všech typů šablon na webu. Pro renderování frontendu je poslední roky snad nejlepší volba použít například React nebo Vue.js, ale třeba pro renderování e-mailových šablon na backendu Latte pořád vítězí.

Jak tedy zajistit render konkrétní HTML šablony do stringu, který můžeme poslat e-mailem?

Snadno:

```php
$latte = new Engine();
$latte->setLoader(new StringLoader());

$template = '<p>Moje jméno je: {$firstName}:{$lastName}!</p>';

$html = $latte->renderToString($template, [
	'firstName' => 'Jan',
	'lastName' => 'Test',
]);

echo $html;
```
