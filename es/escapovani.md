Escapar caracteres en una cadena en PHP
=======================================

> id: '40f9361e-e286-4b5e-a0c0-1f6cda8106af'
> slug:
> 	cs: escapovani
> 	es: escapar-caracteres-en-una-cadena-en-php
> 
> publicationDate: '2019-11-26 11:56:52'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '026284252541bc9c918a87298b9e261c'

La evasión se utiliza para escribir caracteres que tienen diferentes significados en distintos contextos.

Por ejemplo, queremos insertar otra comilla en una cadena encerrada entre comillas. ¿Cómo hacerlo?

Hay dos opciones:

```php
echo "Vaqueros Levi's"; // Combinación de tipos de comillas

echo 'Los vaqueros de Levi\N'; // Escape de la barra invertida
```

Escapar también es importante cuando se escriben variables en una plantilla HTML, donde el contenido de la cadena puede estar en un contexto diferente y significar algo especial.

Por lo tanto, por ejemplo, al enumerar el código HTML (que tenemos en una variable), tenemos que tratar el listado, de lo contrario el código HTML se ejecutará.

Por ejemplo:

```php
$message = 'Hola <b>Tommy!</b>';

echo $message; // ¡Incorrecto!

echo htmlspecialchars($message); // Bien :)
```

El tema de la fuga es muy complejo y recomiendo la lectura del artículo <a href="https://phpfashion.com/escapovani-definitivni-prirucka">La fuga - La guía definitiva</a> de David Grudel.
