Construir incluye
=================

> id: '881cc6ef-b1dc-4a71-ab22-d1943ce8095b'
> slug:
> 	cs: include-function
> 	es: construir-incluye
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '53294e511809a77c08883100fd7416c6'

| Soporte de PHP 4, PHP 5, PHP 7
|---------------|---------
| Descripción breve | Añade otro archivo de texto o script al script.
| Requisitos | Otro archivo de texto o script a insertar.
| Nota | No se pueden cargar archivos externos.

Descripción
--------------------------

Inserta otro archivo de texto o script en la página. Admite archivos de texto plano.

Los archivos incrustados se comportan como si estuvieran directamente en la página.

Las secuencias de comandos incorporadas se ejecutan automáticamente.

El script incrustado transfiere el valor de las variables.

> No se puede cargar desde un almacenamiento externo. Sólo puede leer texto sin formato y archivos PHP.

Formatos de ruta permitidos:

- `script.php` - archivo en el mismo directorio, se ejecutará
- `script.html` - archivo en el mismo directorio, no se ejecutará
- `./file.php` - archivo en el mismo directorio, se ejecutará
- `../página.html`
- Carpeta archivo.php - escribir en Windows
- `dirección/DalsiAddress/file.php` - escribir en sistemas Unix

Las barras del tipo `****` y `**/**` pueden interconvertirse, así que no tienes que lidiar con eso.

Entrada de ruta ilegal: `https://domena.pripona/slozka/soubor.php`

Funciones similares
--------------------------

- <a href="/archivo-obtener-contenido">archivo_obtener-contenido()</a>

Ejemplo
--------------------------

```php
include 'archivo.php';
```

Inserta el script `file.php` en la página y lo ejecuta.

`vars.php`

```php
$color = 'verde';
$fruit = 'manzana';
```

`prueba.php`

```php
$color = '';
$fruit = '';

echo 'A' . $color . '' . $fruit; // A

include 'vars.php';

echo 'A' . $color . '' . $fruit; // Una manzana verde
```

> ADVERTENCIA: La siguiente notación no es posible, ¡transfiere el contenido de las variables definiéndolas!

```php
include 'archivo.php?parámetro=neco';
```

Valores de retorno
--------------------------

Ninguno, sólo pone el archivo.

**NOTA:** Permite comparar el contenido de los archivos, pero esto es un riesgo de seguridad. El orden de los paréntesis es importante. Ejemplo:

```php
if ((include 'archivo.php') == 'OK') {
    echo 'El valor es "OK"';
}
```

> ¡Esto es un riesgo potencial de seguridad!
>
> Se puede resolver con **file_get_contents()**, **readfile()** o **fopen()**. Fopen() sólo debe utilizarse en archivos .txt y .html.
>
> Una lectura más segura del archivo puede resolverse definiendo una función personalizada.

Ejemplo:

```php
$string = get_include_contents('algún archivo.php');

function get_include_contents(string $filename): ?string
{
    if (is_file($filename)) {
        ob_start();
        include (string) $filename;
        return ob_get_clean();
    }

    return null;
}
```

Notas y consejos de otros desarrolladores
--------------------------

**Jakub Vrána** me envió un correo electrónico:

```php
include 'artículos/' . $_GET['Artículo'] . '.html';
```

Esto es extremadamente peligroso.

Un atacante puede pasar un enlace a otro directorio utilizando `../` o algo similar como nombre del artículo, y a veces es posible deshacerse de la terminación pasando un byte nulo al final.

Al menos debería utilizar la función `basename()`, pero es mejor permitir sólo los valores de la lista blanca.
