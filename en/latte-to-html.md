How to render a Latte template to a string
==========================================

> id: '847b028d-c4e6-4480-afe4-797cf83fce81'
> slug:
> 	cs: latte-to-html
> 	en: how-to-render-a-latte-template-to-a-string
> 
> publicationDate: '2022-12-18 11:30:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '0647f751eb754e9cf4afa9f3d0b45e17'

The Latte templating system is suitable for rendering almost all types of templates on the web. For rendering frontend templates, for example, React or Vue.js has been the best choice for the last few years, but for rendering email templates on the backend, Latte still wins.

So how do we ensure we render a specific HTML template to a string that we can send via email?

Easy:

```php
$latte = new Engine();
$latte->setLoader(new StringLoader());

$template = '<p>My name is: {$firstName}:{$lastName}!</p>';

$html = $latte->renderToString($template, [
	'firstName' => 'Jan',
	'lastName' => 'Test',
]);

echo $html;
```
