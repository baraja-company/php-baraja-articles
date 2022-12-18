Sådan gengives en Latte-skabelon til en streng
==============================================

> id: '847b028d-c4e6-4480-afe4-797cf83fce81'
> slug:
> 	cs: latte-to-html
> 	da: sadan-gengives-en-latte-skabelon-til-en-streng
> 
> publicationDate: '2022-12-18 11:30:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '0647f751eb754e9cf4afa9f3d0b45e17'

Latte-skabeloneringssystemet er velegnet til at gengive næsten alle typer skabeloner på nettet. Til rendering af frontend-skabeloner har React eller Vue.js f.eks. været det bedste valg i de sidste par år, men til rendering af e-mail-skabeloner i backend vinder Latte stadig.

Så hvordan sikrer vi, at vi gengiver en bestemt HTML-skabelon til en streng, som vi kan sende via e-mail?

Let:

```php
$latte = new Engine();
$latte->setLoader(new StringLoader());

$template = '<p>Mit navn er: {$fornavn}:{$senavn}!</p>';

$html = $latte->renderToString($template, [
	'fornavn' => 'Jan',
	'efternavn' => 'Test',
]);

echo $html;
```
