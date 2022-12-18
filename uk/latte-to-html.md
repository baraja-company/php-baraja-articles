Як відрендерити шаблон Latte в рядок
====================================

> id: '847b028d-c4e6-4480-afe4-797cf83fce81'
> slug:
> 	cs: latte-to-html
> 	uk: yak-vidrenderiti-sablon-latte-v-ryadok
> 
> publicationDate: '2022-12-18 11:30:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '0647f751eb754e9cf4afa9f3d0b45e17'

Система шаблонів Latte підходить для візуалізації практично всіх типів шаблонів в Інтернеті. Для рендерингу фронтенд-шаблонів, наприклад, React або Vue.js є найкращим вибором протягом останніх кількох років, але для рендерингу email-шаблонів на бекенді, Latte все ще виграє.

Отже, як ми можемо гарантувати, що ми перетворимо певний HTML-шаблон на рядок, який ми можемо відправити електронною поштою?

Легше:

```php
$latte = new Engine();
$latte->setLoader(new StringLoader());

$template = '<p>Мене звуть: {$Ім'я}:{$Прізвище}!</p>';

$html = $latte->renderToString($template, [
	'ім'я та прізвище' => 'Ян',
	'прізвище' => 'Тест',
]);

echo $html;
```
