Archivo_poner_contenido
=======================

> id: '7ec781c6-e3db-484d-8a49-0b93ab8252b1'
> slug:
> 	cs: file-put-contents
> 	es: archivo-poner-contenido
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '78211a3696ebba56d559045f8a0ab9e4'

La función **file_put_contents** es adecuada para escribir automáticamente en un archivo. Como alternativa, también puedes usar <a href="/fopen">fopen()</a>, lo cual no recomiendo para los principiantes.

Muestra
--------------------------

```php
$file = 'archivo.txt';
$content = 'Contenido a guardar en un archivo.';

file_put_contents($file, $content);
```

file_put_contents tiene 2 parámetros:

- `nombre de archivo` donde escribir,
- El `contenido del archivo` que vamos a escribir.

> Nota: `file_put_contents()` sobrescribe el archivo con los últimos contenidos.

Cuidado con la sobreescritura
--------------------------

Si guarda a través de file_put_contents, tenga cuidado con sobrescribir los datos. La función borrará todo el contenido actual y lo sustituirá por el nuevo contenido. Así que si sólo quieres añadir el texto, puedes añadirlo al principio o al final usando tu propio script:

```php
$file = 'archivo.txt';
$content = 'Nuevo contenido.';

$oldContent = file_get_contents($file);
file_put_contents($file, $content . $oldContent);
```

Así que primero se abre el archivo, luego se escribe el nuevo contenido, y el contenido original se escribe después...

Si queremos añadir el contenido antiguo antes del nuevo, sólo tenemos que modificar ligeramente el script:

```php
$file = 'archivo.txt';
$content = Nový obsah.';

$oldContent = file_get_contents($soubor);
file_put_contents($file, $oldContent . $content);
```
