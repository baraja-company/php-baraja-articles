Archivo_obtener_contenido
=========================

> id: '6c8889f1-95e7-4540-9c3a-0225c6383954'
> slug:
> 	cs: file-get-contents
> 	es: archivo-obtener-contenido
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: f8b180a5f7208dab0d37761a5acdc9e3

La función **file_get_contents** se utiliza para leer un archivo y poner su contenido en una variable. Esta función es similar a la función <a href="/include">include</a>, pero a diferencia de include, puede recuperar archivos remotos en Internet y transferir su contenido mediante variables.

Muestra
------

Cualquiera de las dos funciones puede utilizarse para cargar un archivo local desde el disco:

```php
$news = file_get_contents('noticias.html');

echo 'Últimas noticias:<br>' . $news;
```

O desde una URL remota:

```php
$page = file_get_contents('https://www.google.com');

echo $page;
```

Al recuperar una URL, se puede descargar cualquier dirección y recuperar su contenido como una cadena en una variable. En el caso de HTML, se trata del código fuente.

La página se muestra de forma incorrecta
----------------------------

Esto se debe a que el código HTML se pasa exactamente como se coloca en la URL.

Si la ruta de la imagen es, por ejemplo, `<img src="kocka.png">`, es posible que este archivo no exista en el contexto de nuestro servidor, por lo que debemos corregir la ruta a, por ejemplo: `<img src="https://server.cz/kocka.png">`.
