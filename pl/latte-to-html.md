Jak wyrenderować szablon Latte na ciąg znaków
=============================================

> id: '847b028d-c4e6-4480-afe4-797cf83fce81'
> slug:
> 	cs: latte-to-html
> 	pl: jak-wyrenderowac-szablon-latte-na-ciag-znakow
> 
> publicationDate: '2022-12-18 11:30:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '0647f751eb754e9cf4afa9f3d0b45e17'

System szablonów Latte nadaje się do renderowania prawie wszystkich typów szablonów w sieci. Do renderowania szablonów frontendowych, na przykład, React lub Vue.js był najlepszym wyborem przez ostatnie kilka lat, ale do renderowania szablonów e-mailowych na backendzie, Latte nadal wygrywa.

Jak więc upewnić się, że renderujemy określony szablon HTML do łańcucha, który możemy wysłać za pomocą wiadomości e-mail?

Spokojnie:

```php
$latte = new Engine();
$latte->setLoader(new StringLoader());

$template = '<p>Nazywam się: {$firstName}:{$lastName}!</p>';

$html = $latte->renderToString($template, [
	'firstName' => 'Jan',
	'lastName' => 'Test',
]);

echo $html;
```
