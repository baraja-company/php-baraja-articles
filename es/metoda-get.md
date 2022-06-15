Obtención de parámetros de la URL mediante el método GET
========================================================

> id: bbf2cb2c-f7f7-4be9-a9cd-960014db0f51
> slug:
> 	cs: metoda-get
> 	es: obtencion-de-parametros-de-la-url-mediante-el-metodo-get
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: caebe20b6b7cdf0b3703ff55dad6f094

Ya sabes, tienes una página abierta, sigues la URL y ves un signo de interrogación con algunos parámetros. Un programador inexperto pensaría que se trata de archivos separados, pero he aquí que. Intenta crear un archivo que tenga un signo de interrogación en su nombre (no funciona). **Esta es la razón por la que se escribió este artículo**.

¿Qué es?
--------------------------

En realidad, la cuestión es que es un único archivo al que le pasas variables a través de una URL, así que tengo, por ejemplo, un archivo **index.php**, y le paso el nombre del artículo: **index.php?clanek=o-php**.

Código + explicación
--------------------------

La variable superglobal `$_GET` contiene claves con parámetros de la URL

```php
echo $_GET['Artículo'] ?? '';
```

Límites de seguridad y longitud
--------------------------

El método GET no es seguro, por lo que no se deben enviar datos confidenciales a través de él, una de las principales razones es que se trata de una comunicación no cifrada y en segundo lugar se almacena en el historial.

Los datos confidenciales o simplemente todo debe enviarse mediante el método <a href="/método-post">POST</a>. GET es más adecuado para furmulares donde es bueno mostrar parámetros (como motores de búsqueda, página de artículos) para que la página pueda ser enlazada.

La duración del GET no es ilimitada. Muchos principiantes pagan por esto. La longitud máxima es de unos 1024 caracteres (en algunos sitios dicen 1088). Así que para textos más largos, envía <a href="/método-post">POST</a>em.
