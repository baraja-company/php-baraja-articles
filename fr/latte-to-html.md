Comment convertir un modèle de latte en une chaîne de caractères ?
==================================================================

> id: '847b028d-c4e6-4480-afe4-797cf83fce81'
> slug:
> 	cs: latte-to-html
> 	fr: comment-convertir-un-modele-de-latte-en-une-chaine-de-caracteres
> 
> publicationDate: '2022-12-18 11:30:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '0647f751eb754e9cf4afa9f3d0b45e17'

Le système de modélisation Latte permet de rendre presque tous les types de modèles sur le Web. Pour le rendu des modèles en front-end, par exemple, React ou Vue.js est le meilleur choix depuis quelques années, mais pour le rendu des modèles d'e-mails en back-end, Latte l'emporte toujours.

Alors comment s'assurer que nous rendons un modèle HTML spécifique en une chaîne de caractères que nous pouvons envoyer par courriel ?

Facile :

```php
$latte = new Engine();
$latte->setLoader(new StringLoader());

$template = '<p>Mon nom est : {$firstName}:{$lastName}!</p>';

$html = $latte->renderToString($template, [
	'premierNom' => 'Jan',
	'Nom de famille' => 'Test',
]);

echo $html;
```
