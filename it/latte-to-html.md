Come rendere un modello Latte in una stringa
============================================

> id: '847b028d-c4e6-4480-afe4-797cf83fce81'
> slug:
> 	cs: latte-to-html
> 	it: come-rendere-un-modello-latte-in-una-stringa
> 
> publicationDate: '2022-12-18 11:30:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '0647f751eb754e9cf4afa9f3d0b45e17'

Il sistema di template Latte è adatto a rendere quasi tutti i tipi di template sul web. Per il rendering dei modelli di frontend, ad esempio, React o Vue.js sono stati la scelta migliore negli ultimi anni, ma per il rendering dei modelli di e-mail sul backend, Latte vince ancora.

Quindi, come possiamo assicurarci di rendere un modello HTML specifico in una stringa che possiamo inviare via e-mail?

Facile:

```php
$latte = new Engine();
$latte->setLoader(new StringLoader());

$template = '<p>Il mio nome è: {$firstName}:{$lastName}!</p>';

$html = $latte->renderToString($template, [
	'nomeNome' => 'Gennaio',
	'cognome' => 'Test',
]);

echo $html;
```
