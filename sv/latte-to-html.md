Så här gör du en Latte-mall till en sträng
==========================================

> id: '847b028d-c4e6-4480-afe4-797cf83fce81'
> slug:
> 	cs: latte-to-html
> 	sv: sa-haer-goer-du-en-latte-mall-till-en-straeng
> 
> publicationDate: '2022-12-18 11:30:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '0647f751eb754e9cf4afa9f3d0b45e17'

Latte-systemet för mallar är lämpligt för att återge nästan alla typer av mallar på webben. För rendering av mallar på fronten har till exempel React eller Vue.js varit det bästa valet under de senaste åren, men för rendering av e-postmallar på baksidan vinner Latte fortfarande.

Så hur säkerställer vi att vi render en specifik HTML-mall till en sträng som vi kan skicka via e-post?

Enkelt:

```php
$latte = new Engine();
$latte->setLoader(new StringLoader());

$template = '<p>Mitt namn är: {$förnamn}:{$efternamn}!</p>';

$html = $latte->renderToString($template, [
	'förnamn' => 'Jan',
	'Efternamn' => 'Test',
]);

echo $html;
```
