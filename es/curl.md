cURL en PHP - descarga de datos vía URL
=======================================

> id: '0bd1aed6-460d-4b63-9afe-f5087d1c6046'
> slug:
> 	cs: curl
> 	es: curl-en-php-descarga-de-datos-via-url
> 
> perex:
> 	- Knihovna cURL je robustní PHP knihovna pro pokročilou HTTP komunikaci.
> 	- La librería cURL es una robusta librería PHP para la comunicación HTTP avanzada.
> 
> publicationDate: '2020-02-15 22:14:32'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '7c565449293cf1ce4950f309abaf04d0'

La biblioteca de PHP `cURL` es una buena manera de descargar datos de un servidor extranjero.

A partir de una consulta, construye una petición HTTP que envía al servidor de destino y, una vez descargada, contiene una API para el manejo (relativamente) sencillo de los datos.

A diferencia de la función nativa `file_get_contents` (a través de la cual también podemos realizar peticiones HTTP), ofrece unas opciones de configuración mucho mejores y descarga páginas/archivos como un navegador real.

> La función `file_get_contents` utiliza internamente la librería `cURL`, sólo que no tiene opciones de configuración tan detalladas.

Detección del modo cURL en una solicitud
----------------------------

A menudo es útil detectar si la solicitud actual se hizo a través de `cUrl` o clásicamente en el navegador.

No hay una implementación directa para esto en PHP, pero podemos escribir una función simple nosotros mismos:

```php
function isCurl(): bool
{
    return str_contains($_SERVER['HTTP_USER_AGENT'] ?? '', 'rizo');
}
```

Si tienes Linux y su Terminal, o estás en un Mac, prueba este comando:

```shell
curl https://php.baraja.cz/curl
```

El comando hace una petición interna a este sitio y devuelve el resultado.

Si la aplicación no detecta una petición cURL, el HTML se devolverá como si la petición procediera del navegador. Sin embargo, dado que se detectan los tipos de solicitud, nada impide que devolvamos un artículo Markdown depurado.

La ventaja es que los datos están mucho más limpios. Mostramos el HTML formateado al usuario en el navegador, pero sólo el contenido básico al robot.

Uso detallado de la API en PHP
--------------------------

Si buscas un uso detallado de cUrl, te recomiendo que leas la <a href="https://www.php.net/manual/en/book.curl.php">documentación oficial</a>, que siempre está actualizada.

Para un uso casual, existe una biblioteca <a href="https://docs.guzzlephp.org/en/stable/">**Guzzle**</a> que maneja
