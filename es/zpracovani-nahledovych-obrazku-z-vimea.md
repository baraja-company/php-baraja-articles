Procesamiento de imágenes en miniatura y meta información de Vimeo
==================================================================

> id: '8b8cbef0-210a-4883-be07-5004e8f43739'
> slug:
> 	cs: zpracovani-nahledovych-obrazku-z-vimea
> 	es: procesamiento-de-imagenes-en-miniatura-y-meta-informacion-de-vimeo
> 
> publicationDate: '2020-09-19 17:32:31'
> mainCategoryId: a0143f3c-ac75-46dc-a514-d3c9417ded4e
> sourceContentHash: '93c29218f162fb5dc8d0f265968b0f8d'

Al incrustar vídeos de Vimeo en una página (como incrustación HTML), a menudo podemos querer obtener también la imagen y otra información útil como la duración del vídeo, el título completo, el autor, etc.

Afortunadamente, Vimeo proporciona una sencilla API HTTP desde la que podemos leer todos los datos basados en el token del vídeo.

Para evitar tener que escribir la API usted mismo, sólo tiene que utilizar [paquete listo](https://github.com/baraja-core/vimeo-video-api), que integra la API por completo.

El paquete se instala con el comando

```shell
composer require baraja-core/vimeo-video-api
```

Es fácil de usar. Se crea una instancia del servicio `Baraja\VimeoAPI\VimeoVideoAPI` para comunicarse con Vimeo según la documentación, se llama al método `getInfo()`, se pasa el token del vídeo y se obtiene información detallada como una entidad `VideoInfo` de la que se puede leer toda la información disponible (no siempre está disponible toda la información para cada vídeo).

De este modo, puedes consultar incluso los vídeos privados y los no disponibles públicamente. Pero siempre hay que conocer su ficha.

Listado de toda la información disponible
---------

Una forma básica de utilizar la biblioteca es la siguiente:

```php
$api = new \Baraja\VimeoAPI\VimeoVideoAPI;

$token = 0; // Ficha de vídeo en forma de número entero
$info = $api->getInfo($token);

echo var_dump($info); // lista todo

// Imprime la duración del vídeo en segundos:
echo 'La duración del vídeo es:' . $info->getDuration();
```

La variable `$info` almacena toda la información descriptiva de un vídeo concreto. Un resumen de todos los métodos disponibles [se puede encontrar en la aplicación](https://github.com/baraja-core/vimeo-video-api/blob/master/src/VideoInfo.php).
