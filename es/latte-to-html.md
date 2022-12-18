Cómo convertir una plantilla Latte en una cadena
================================================

> id: '847b028d-c4e6-4480-afe4-797cf83fce81'
> slug:
> 	cs: latte-to-html
> 	es: como-convertir-una-plantilla-latte-en-una-cadena
> 
> publicationDate: '2022-12-18 11:30:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '0647f751eb754e9cf4afa9f3d0b45e17'

El sistema de plantillas Latte es adecuado para renderizar casi todo tipo de plantillas en la web. Para renderizar plantillas frontend, por ejemplo, React o Vue.js han sido la mejor opción durante los últimos años, pero para renderizar plantillas de correo electrónico en el backend, Latte sigue ganando.

Entonces, ¿cómo nos aseguramos de convertir una plantilla HTML específica en una cadena que podamos enviar por correo electrónico?

Fácil:

```php
$latte = new Engine();
$latte->setLoader(new StringLoader());

$template = '<p>Mi nombre es: {$firstName}:{$lastName}!</p>';

$html = $latte->renderToString($template, [
	'firstName' => 'Jan',
	'apellido' => 'Prueba',
]);

echo $html;
```
