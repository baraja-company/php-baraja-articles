Formularios HTML - parte en el navegador
========================================

> id: cb0015c7-b7b6-41ac-8263-4068960e16b3
> slug:
> 	cs: formulare
> 	es: formularios-html-parte-en-el-navegador
> 
> perex:
> 	- 'Zpracování formulářů v PHP, zejména možnosti odeslání získaných dat na server metodou GET a POST.'
> 	- 'Procesamiento de formularios en PHP, especialmente la posibilidad de enviar los datos obtenidos al servidor mediante los métodos GET y POST.'
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '8d2a0df0667b3da5d6b19e6c25ad2517'

Antes de que podamos procesar cualquier dato del usuario en el lado del servidor a través de PHP, necesitamos obtenerlo primero. Esto se hace en el navegador mediante formularios HTML que definen los elementos básicos para recibir los datos. El propósito de este artículo no es presentar todas las posibilidades de formas, sino sólo las posibilidades básicas de aceptar datos y entender el principio.

Fuente del formulario HTML básico
-----------------------------

```html
<form action="script.php" method="get">

<!-- Zde bude celý obsah formuláře -->

</form>
```

Cada formulario comienza con la etiqueta HTML `<form>` y termina con la etiqueta `</form>`. Todos los campos de formulario colocados entre estas etiquetas serán enviados.

A continuación, debes establecer a dónde enviar el formulario con el atributo `action` (nombre del script), y con qué método enviarlo con el atributo `method` (GET o POST). Si no especificas un método y un destino, el formulario se envía por defecto con el método GET.

Campos de formulario básicos
-------------------------

El campo más usado se utiliza para obtener el texto (cadena). Cada campo tiene su propio tipo y nombre por el que puede ser reconocido después del envío.

Campos de texto comunes
------------------

Lo más importante es que necesito un campo de texto sin formato:

```html
<input type="text" name="food">
```

<input type="text" name="food">

Campo de la contraseña
---------------------

```html
<input type="password" name="heslo">
```

<input type="password" name="password">

Casilla de verificación
--------

Se utiliza para comprobar el booleano (`TRUE` y `FALSE`):

```html
<input type="checkbox" name="vop" checked="checked">
```

<label>
	<input type="checkbox" name="vop" checked="checked">¿Acepta las condiciones?
</label>

Botón de radio para seleccionar varias opciones
------------------------------------

```html
<input type="radio" name="language" value="cz" checked> Čeština
<input type="radio" name="language" value="sk"> Slovenština
<input type="radio" name="language" value="en"> Angličtina
```

Le permite elegir entre varias opciones. La opción seleccionada envía su valor. Por defecto es bueno seleccionar un campo con el atributo `checked="checked"`:

<label>
	<input type="radio" name="language" value="cz" checked="checked"> Checo
</label><br>
<label>
	<input type="radio" name="language" value="en"> Eslovaco
</label><br>
<label>
	<input type="radio" name="language" value="en"> Español
</label>

Campo de texto grande
------------------

Creado para introducir texto de varias líneas. También se utiliza para entrar:

- `cols` ~ número de columnas
- `rows` ~ número de filas

```html
<textarea name="article" cols="40" rows="6">
Ahoj lidi!
</textarea>
```

<textarea name="article" cols="40" rows="6">
¡Hola chicos!
</textarea>

Caja de selección
---------

Presenta una forma cómoda de seleccionar entre muchos datos.

```html
<select name="gender">
	<option value="man">Muž</option>
	<option value="woman">Žena</option>
</select>
```

Después de enviar el formulario, se envía el valor en `value`.

<select name="gender">
	<opción value="man">Hombre</opción>
	<opción value="woman">Mujer</opción>
</selección>

Botón de envío
---------------------

El formulario puede tener un número ilimitado de botones de envío. Son fáciles de introducir:

```html
<input type="submit" value="Odeslat">
```

Al hacer clic, toma todos los datos de los campos del formulario y los envía al script establecido:

<input type="submit" value="Submit">

Procesamiento de datos en el servidor
-------------------------

A continuación, hay que enviar los datos al servidor y procesarlos allí, esto se trata en <a href="/processing-formula-in-php">el siguiente artículo</a>.
