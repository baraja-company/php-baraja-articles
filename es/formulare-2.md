Formularios, procesamiento de formularios en PHP
================================================

> id: d1871a8d-bcfe-408d-ac12-6b827633f77e
> slug:
> 	cs: formulare-2
> 	es: formularios-procesamiento-de-formularios-en-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '91f43ee06328e88a6a1058ecbf028222'

Supongo que hemos creado un formulario HTML, que enviamos y ahora queremos procesar los datos. Hay un <a href="/formulare">artículo aparte</a> sobre la creación de un formulario HTML.

Recepción de datos - diferentes formas
----------------------------

La forma de enviar el formulario se establece directamente en el HTML

Hay dos opciones:

- **GET** - Es visible en la barra de direcciones después del signo de interrogación
 Por ejemplo: `php.baraja.cz/search.php?query=formulare`
- **POST** - Oculto (no visible), la mayoría de los formularios se envían por correo

Luego tenemos que leerlos en PHP utilizando el mismo método.

Obtener los datos del usuario y transferirlos al script
------------------------------------------------------

La base es un formulario HTML, cómo hacerlo puedes leer <a href="/formulare">en un artículo aparte</a>.

Para empezar, supongamos un simple formulario para introducir el nombre del usuario:

```html
<form action="welcome.php" method="GET">
   Zadejte jméno: <input type="text" name="username">
   <input type="submit" value="odeslat">
</form>
```

Aparecerá un cuadro de texto para introducir un nombre y hacer clic para enviar. Cuando se hace clic en el botón, el contenido del campo se envía al script `welcome.php`.

Ahora el procesamiento real en el archivo `welcome.php`:

```php
$username = $_GET['nombre de usuario'];

echo 'El nombre introducido es:' . $username;
```

Observe la variable especial `$_GET`. Es una **variable superglobal** que contiene datos del formulario y a la que se puede acceder como un array.

El problema con esta solución, sin embargo, es que los datos recibidos no son **seguros** y una forma similar puede ser fácilmente atacada. Por ejemplo, un atacante potencial puede introducir un código javascript en el campo en lugar de un nombre, que se escribirá en la página y se ejecutará.

Por lo tanto, siempre debemos desinfectar cualquier dato del usuario antes de imprimirlo en el código HTML:

```php
$username = $_GET['nombre de usuario'] ?? 'Desconocido';

echo 'El nombre introducido es:' . htmlspecialchars($username);
```

Tratamiento posterior
----------------

Podemos hacer cualquier cosa con los datos recibidos y tratarlos como cualquier variable ordinaria.

Por ejemplo, añada el valor en dos campos:

```php
echo $_GET['x'] + $_GET['y'];
```

O guardar en un archivo, base de datos, correo electrónico, ...

Las siguientes funciones son útiles para ello:

- <a href="/archivo-poner-contenidos">archivo_poner-contenidos</a> - función para guardar datos en un archivo
- <a href="/hashovani">MD5</a> - cálculo de la suma de comprobación, por ejemplo para las contraseñas
- <a href="/cookies">Cookies</a> - guardar datos en **cookies** (pequeños archivos dentro del navegador web)
