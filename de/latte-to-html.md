Wie man eine Latte-Vorlage in eine Zeichenkette umwandelt
=========================================================

> id: '847b028d-c4e6-4480-afe4-797cf83fce81'
> slug:
> 	cs: latte-to-html
> 	de: wie-man-eine-latte-vorlage-in-eine-zeichenkette-umwandelt
> 
> publicationDate: '2022-12-18 11:30:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '0647f751eb754e9cf4afa9f3d0b45e17'

Das Latte-Vorlagensystem eignet sich für die Darstellung fast aller Arten von Vorlagen im Web. Für das Rendering von Frontend-Templates zum Beispiel sind React oder Vue.js seit einigen Jahren die beste Wahl, aber für das Rendering von E-Mail-Templates im Backend ist Latte immer noch die beste Wahl.

Wie können wir also sicherstellen, dass wir eine bestimmte HTML-Vorlage in eine Zeichenkette umwandeln, die wir per E-Mail versenden können?

Einfach:

```php
$latte = new Engine();
$latte->setLoader(new StringLoader());

$template = '<p>Mein Name ist: {$Vorname}:{$Nachname}!</p>';

$html = $latte->renderToString($template, [
	'Vorname' => 'Jan',
	'Nachname' => 'Test',
]);

echo $html;
```
