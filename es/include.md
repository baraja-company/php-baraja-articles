PHP Include - insertar un archivo en una página
===============================================

> id: '7a53145c-8552-425d-b864-283f73a7a7de'
> slug:
> 	cs: include
> 	es: php-include-insertar-un-archivo-en-una-pagina
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '6e18e67ec03b3a7592473230559fc364'

La construcción `include` inserta automáticamente archivos/scripts adicionales en la página actual.

En lo que respecta a PHP, se ejecutará automáticamente y se entenderá como si siempre hubiera estado en esa ubicación.

Ejemplo:

```php
include 'noticias.html';
```

Uso práctico
-----------------

- Menciones y menús comunes,
- Noticias, actualizaciones, novedades y el mismo contenido,
- Cabecera o pie de página, ...

Carga dinámica de archivos
--------------------------

A menudo necesitamos cargar archivos de forma dinámica, por ejemplo, en función de una variable.

Por ejemplo:

```php
$clanek = 'neco';

include $clanek . '.html';
```

O podemos cargar dinámicamente los artículos en la página:

```php
include 'artículos/' . $_GET['página'] . '.html';
```

Cuando se llama a una URL con el parámetro `page`, el artículo se inserta automáticamente, por ejemplo: `https://example.com/?page=novinky`.
