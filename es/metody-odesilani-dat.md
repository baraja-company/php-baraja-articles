Métodos de envío de datos (GET y POST)
======================================

> id: '32f9083f-7cb1-469f-911a-765df059123d'
> slug:
> 	cs: metody-odesilani-dat
> 	es: metodos-de-envio-de-datos-get-y-post
> 
> perex:
> 	- 'Metoda GET a POST, získávání dat z formuláře a URL. Komunikace přes API a zpracování dat.'
> 	- 'Métodos GET y POST, recuperación de datos de formularios y URL. Comunicación API y tratamiento de datos.'
> 
> publicationDate: '2019-11-26 11:38:32'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '81b5f92d7ee05563b6ece295ed5958d3'

Además de las variables normales, también tenemos las llamadas **variables superglobales** en PHP, que llevan información sobre la página a la que se llama en ese momento y los datos que pasamos.

Típicamente, tenemos un formulario en una página que el usuario rellena y queremos transferir esos datos al servidor web donde los procesamos en PHP.

Los métodos más utilizados para ello son tres:

- `GET` ~ los datos se pasan en la URL como parámetros
- `POST` ~ los datos se pasan de forma encubierta junto con la petición de página
- <a href="/ajax-post">Ajax POST</a> ~ procesamiento asíncrono javascript

Método GET - `$_GET`
--------------------

Los datos enviados por el método GET son visibles en la URL (como parámetros después del signo de interrogación), la longitud máxima es de 1024 caracteres en Internet Explorer (otros navegadores *casi* no lo restringen, pero los textos más grandes no deberían pasarse de esta forma). La ventaja de este método es sobre todo la sencillez (puede ver lo que está enviando) y la posibilidad de proporcionar un enlace al resultado del tratamiento. Los datos se envían a una variable.

La dirección de la página receptora podría tener este aspecto:

`https://____________.com/script.php?promenna=obsah&promenna2=obsah`

En PHP, podemos entonces, por ejemplo, escribir el valor del parámetro `variable` de la siguiente manera:

```php
echo $_GET['paseo marítimo'];	// imprime "contenido"
```

> **Atención:** Este método de escribir datos directamente en la página HTML no es seguro, porque podemos pasar, por ejemplo, código HTML en la URL que sería escrito en la página y luego ejecutado.
>
> Debemos **siempre** tratar los datos antes de cualquier salida a la página, para ello se utiliza la función `htmlspecialchars()`.
>
> Por ejemplo: `echo htmlspecialchars($_GET['variable']);`

Método POST - `$_POST`.
----------------------

Los datos enviados por el método POST no son visibles en la URL, lo que resuelve el problema de la longitud máxima de los datos enviados. Siempre debe utilizarse el método POST para enviar campos de formulario, ya que así se garantizará que, por ejemplo, las contraseñas no sean visibles y que no se pueda proporcionar un enlace a la página que procesa el resultado de una determinada entrada.

Los datos están disponibles en la variable `$_POST` y el uso es el mismo que para el método GET.

Verificación de la existencia de los datos presentados
--------------------------------

Antes de procesar cualquier dato, primero debemos verificar que los datos se han enviado realmente, de lo contrario estaríamos accediendo a
 a una variable inexistente, lo que arrojaría un mensaje de error.

La función `isset()` se utiliza para verificar la existencia de una variable.

```php
if (isset($_GET['Nombre'])) {
    echo 'Tu nombre:' . htmlspecialchars($_GET['Nombre']);
} else {
    echo 'No se ha introducido ningún nombre.';
}
```

Formulario de introducción de datos
------------------------

El formulario está hecho en HTML, no en PHP. Puede ser en una simple página HTML. Toda la "magia" es manejada por el script PHP que acepta los datos.

Por ejemplo, podemos utilizar un formulario para recibir 2 números, enviados por el método GET:

```html
<form action="script.php" method="get">
    První číslo: <input type="text" name="x">
    Druhé číslo: <input type="text" name="y">

    <input type="submit" value="Sečíst čísla">
</form>
```

En la primera línea puede ver dónde se enviarán los datos y mediante qué método.

Las siguientes 2 líneas son simples elementos de formulario, note el atributo **name=""**, allí está el nombre de la variable que contendrá lo que ahora está en el formulario.

A continuación viene el botón para enviar los datos (obligatorio) y la etiqueta HTML de cierre del formulario (obligatoria para que el navegador sepa qué más enviar y qué no).

> Podemos tener cualquier número de formularios en una página, y no pueden estar anidados. Si se produce anidamiento, siempre se envía el formulario más anidado y se ignoran los demás.

Tratamiento de formularios en el servidor
-------------------------------

Ahora tenemos un formulario HTML terminado y lo enviamos a `script.php`, que recibe los datos usando el método GET. La dirección de la solicitud de la página podría tener este aspecto:

`https://________.com/script.php?x=5&y=3`

**script.php**

```php
$x = $_GET['x'];	// 5
$y = $_GET['y'];	// 3

echo $x + $y;		// imprime 8
```

Correctamente, primero debemos verificar que ambos campos del formulario han sido rellenados, esto se hace con la función `isset()`:

```php
if (isset($_GET['x']) && isset($_GET['y'])) {
    $x = $_GET['x'];	// 5
    $y = $_GET['y'];	// 3

    echo $x + $y;		// imprime 8
} else {
    echo 'El formulario no se ha rellenado correctamente.';
}
```

> **TIP:** Puedes pasar múltiples parámetros a la construcción `isset()` para verificar que todos ellos existen.
>
> Por lo tanto, en lugar de `isset($_GET['x']) && isset($_GET['y'])`, puede simplemente especificar:
>
> `isset($_GET['x'], $_GET['y'])`.

Tratamiento de los datos recibidos por el método POST
--------------------------------------

Si los datos se reciben por el método POST, la URL del script a procesar siempre tendrá este aspecto:

`https://________.com/script.php`

Y nunca de otra manera. Simplemente no. Los datos están ocultos en la petición HTTP y no podemos verlos.

> El método POST oculto es necesario para enviar nombres de usuario y contraseñas por razones de seguridad.
>
> **Seguridad:** Si trabajas con contraseñas en tu sitio, el formulario de acceso y registro debe estar alojado en HTTPS y debes hashear las contraseñas adecuadamente (por ejemplo, con BCrypt).

Gestión de solicitudes ajax
------------------------------

En algunos casos, al procesar peticiones ajax, puede que no sea fácil recuperar los datos. Esto se debe a que las bibliotecas ajax suelen enviar datos como `json payload`, mientras que la variable superglobal `$_POST` contiene sólo datos de formulario.

Todavía se puede acceder a los datos, he descrito los detalles en el artículo <a href="/ajax-post">Manejo de peticiones ajax POST</a>.

Obtención de datos brutos
-----------------------------

A veces puede ocurrir que un usuario envíe una petición utilizando un método HTTP inapropiado, y añada su propia entrada encima. O, por ejemplo, envía un archivo binario, o malas cabeceras HTTP.

Para tal caso, es bueno utilizar la entrada nativa, que se obtiene en PHP de la siguiente manera:

```php
$input = file_get_contents('php://entrada');
```

Mientras implementaba la biblioteca API REST, también me encontré con una serie de casos especiales en los que diferentes tipos de servidores web decidían incorrectamente las cabeceras HTTP de entrada, o el usuario enviaba incorrectamente los datos del formulario, etc.

Para este caso, pude implementar esta función que resuelve casi todos los casos (la implementación depende de `Nette\Http\RequestFactory`, pero puedes reemplazar esta dependencia por otra en tu proyecto específico):

```php
/**
 * Obtiene los datos POST directamente de la cabecera HTTP, o intenta parsear los datos de la cadena.
 * Algunos clientes heredados envían datos como json, que está en formato de cadena base, por lo que la conversión de campos a array es obligatoria.
 *
 * @return array<string|int, mixed>
 */
private function getBodyParams(string $method): array
{
	if ($method === 'GET' || $method === 'BORRAR') {
		return [];
	}

	$request = (new RequestFactory())->fromGlobals();
	$return = array_merge((array) $request->getPost(), $request->getFiles());
	try {
		$post = array_keys($_POST)[0] ?? '';
		if (str_starts_with($post, '{') && str_ends_with($post, '}')) { // compatibilidad con clientes heredados
			$json = json_decode($post, true, 512, JSON_THROW_ON_ERROR);
			if (is_array($json) === false) {
				throw new LogicException('Json no es un array válido.');
			}
			unset($_POST[$post]);
			foreach ($json as $key => $value) {
				$return[$key] = $value;
			}
		}
	} catch (Throwable $e) {
		// El silencio es oro.
	}
	try {
		$input = (string) file_get_contents('php://entrada');
		if ($input !== '') {
			$phpInputArgs = (array) json_decode($input, true, 512, JSON_THROW_ON_ERROR);
			foreach ($phpInputArgs as $key => $value) {
				$return[$key] = $value;
			}
		}
	} catch (Throwable $e) {
		// El silencio es oro.
	}

	return $return;
}
```
