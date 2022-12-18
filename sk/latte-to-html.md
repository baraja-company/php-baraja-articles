Ako vykresliť šablónu Latte do reťazca
======================================

> id: '847b028d-c4e6-4480-afe4-797cf83fce81'
> slug:
> 	cs: latte-to-html
> 	sk: ako-vykreslit-sablonu-latte-do-retazca
> 
> publicationDate: '2022-12-18 11:30:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '0647f751eb754e9cf4afa9f3d0b45e17'

Šablónovací systém Latte je vhodný na vykresľovanie takmer všetkých typov šablón na webe. Napríklad na vykresľovanie šablón na frontende je už niekoľko rokov najlepšou voľbou React alebo Vue.js, ale na vykresľovanie e-mailových šablón na backende stále vyhráva Latte.

Ako teda zabezpečíme vykreslenie konkrétnej šablóny HTML do reťazca, ktorý môžeme odoslať e-mailom?

Jednoduché:

```php
$latte = new Engine();
$latte->setLoader(new StringLoader());

$template = '<p>Môj názov je: {$firstName}:{$lastName}!</p>';

$html = $latte->renderToString($template, [
	'meno' => 'Jan',
	'priezvisko' => 'Test',
]);

echo $html;
```
