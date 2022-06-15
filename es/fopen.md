Función PHP fopen()
===================

> id: '733ba757-1d5f-4717-a088-5ddabe730838'
> slug:
> 	cs: fopen
> 	es: funcion-php-fopen
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '6b791877e57d88840d603dea69967b69'

La función `fopen()` representa el acceso de bajo nivel a los archivos en el disco.

El programador tiene que hacerlo todo él mismo (abrir el archivo, leer los datos, escribir nuevos datos, cerrar el archivo).

Si sólo necesitas leer y escribir archivos rápidamente, hay opciones más sencillas:

- <a href="/file-get-contents">file_get_contents()</a> - lectura de un archivo
- <a href="/archivo-poner-contenido">archivo_poner-contenido()</a> - escribir en un archivo

Uso básico
----------------

```php
$text = 'Cualquier texto que se guarde...';

$file = fopen('archivo.html', 'a+'); // Abre el archivo y el modo

fwrite($file, $text); // Guarda en un archivo
fclose($file);        // Cierra el archivo
```

> Si abrimos un archivo para su lectura y no se cierra, ¡ningún otro proceso podrá acceder a él!

Tipos de modos de manejo de archivos
----------------------------

Podemos trabajar con archivos en diferentes modos, que nos informan sobre los derechos de acceso.

Por ejemplo, si queremos abrir un archivo de sólo lectura, entonces el modo `r` es suficiente.

Si abrimos el archivo para escribir, se marcará como `open` en el disco y otro proceso (script) no podrá escribir en él hasta que lo cerremos de nuevo. Esto garantiza que el archivo no se corrompa durante la escritura.

| Modo, significado.
|-------|--------|
| `y` | Abre el archivo, si no existe se creará |
| `a+` | Abre un archivo para añadir datos y o leer datos, si no existe se creará |
| `r` | Abrir sólo lectura |
| | R+` | Abrir para leer y escribir |
| `w` | Abrir para escribir, los datos originales serán borrados y sustituidos por los nuevos, si no existen serán creados |
| `w+` | Abrir para escribir y leer, los datos originales serán borrados y sustituidos por los nuevos, si no existen serán creados |
