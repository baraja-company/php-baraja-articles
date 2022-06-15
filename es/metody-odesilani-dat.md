Métodos de envío de datos (GET y POST)
======================================

> id: '32f9083f-7cb1-469f-911a-765df059123d'
> slug:
> 	cs: metody-odesilani-dat
> 	es: metodos-de-envio-de-datos-get-y-post
> 
> perex:
> 	- 'Metoda GET a POST, získávání dat z formuláře a URL. Komunikace přes API a zpracování dat.'
> 	- 'Método GET y POST, recuperación de datos desde el formulario y la URL. Comunicación y procesamiento de datos de la API.'
> 
> publicationDate: '2019-11-26 11:38:32'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '2282538597ebfe95877ae0e005ddd352'

Además de las variables regulares, también tenemos las llamadas **variables superglobales** en PHP, que llevan información sobre la página llamada actualmente y los datos que estamos pasando.

Normalmente, tenemos un formulario en una página que el usuario rellena y queremos transferir esos datos al servidor web donde los procesamos en PHP.

Hay tres métodos más utilizados para hacerlo:

- `GET` ~ los datos se pasan en la URL como parámetros
- `POST` ~ los datos se pasan de forma encubierta junto con la solicitud de la página
- <a href="/ajax-post">Ajax POST</a> ~ procesamiento asíncrono de javascript

Método GET - `$_GET`.
--------------------

Los datos enviados por el método GET son visibles en la URL (como parámetros después del signo de interrogación), la longitud máxima es de 1024 caracteres en Internet Explorer (otros navegadores *casi* no lo restringen, pero los textos más grandes no deberían pasarse de esta manera). La ventaja de este método es sobre todo la simplicidad (se puede ver lo que se envía) y la posibilidad de proporcionar un enlace al resultado del tratamiento. Los datos se envían a una variable.

La dirección de la página receptora podría tener el siguiente aspecto:

`https://____________.com/script.php?promenna=obsah&promenna2=obsah`

En PHP, podemos entonces, por ejemplo, escribir el valor del parámetro `variable` de la siguiente manera:

```php
echo $_GET['paseo marítimo'];	// imprime "contenido"
```

> **Atención: ** Este método de escribir datos directamente en la página HTML no es seguro, porque podemos pasar, por ejemplo, código HTML en la URL que se escribiría en la página y luego se ejecutaría.
>
> Debemos **siempre** tratar los datos antes de cualquier salida a la página, para ello se utiliza la función `htmlspecialchars()`.
>
> Por ejemplo: `echo htmlspecialchars($_GET['variable']);`

Método POST - `$_POST`
----------------------

Los datos enviados por el método POST no son visibles en la URL, lo que resuelve el problema de la longitud máxima de los datos enviados. El método POST debe usarse siempre para enviar los campos de los formularios, ya que así se garantiza que, por ejemplo, las contraseñas no sean visibles y que no se pueda proporcionar un enlace a la página que procesa el resultado de una entrada determinada.

Los datos están disponibles en la variable `$_POST` y el uso es el mismo que para el método GET.

Verificación de la existencia de los datos presentados
--------------------------------

Antes de procesar cualquier dato, primero debemos verificar que los datos han sido realmente enviados, de lo contrario estaríamos accediendo a
 a una variable inexistente, lo que arrojaría un mensaje de error.

La función `isset()` se utiliza para verificar la existencia de una variable.

```php
if (isset($_GET['Nombre'])) {
    echo 'Su nombre:' . htmlspecialchars($_GET['Nombre']);
} else {
    echo 'No se ha introducido ningún nombre.';
}
```

Formulario de entrada de datos
------------------------

El formulario está hecho en HTML, no en PHP. Puede ser en una página HTML simple. Toda la "magia" es manejada por el script PHP que acepta los datos.

Por ejemplo, podemos utilizar un formulario para recibir 2 números, enviados por el método GET:

```html
<form action="script.php" method="get">
    První číslo: <input type="text" name="x">
    Druhé číslo: <input type="text" name="y">

    <input type="submit" value="Sečíst čísla">
</form>
```

En la primera línea se puede ver dónde se enviarán los datos y por qué método.

Las siguientes 2 líneas son simples elementos de formulario, note el atributo **name=""**, allí está el nombre de la variable que contendrá lo que ahora está en el formulario.

A continuación viene el botón para enviar los datos (obligatorio) y la etiqueta HTML de cierre del formulario (obligatoria para que el navegador sepa qué más debe enviar y qué no).

> Podemos tener cualquier número de formularios en una página, y no pueden estar anidados. Si se produce el anidamiento, siempre se envía el formulario más anidado y se ignoran los demás.

Procesamiento de formularios en el servidor
-------------------------------

Ahora tenemos un formulario HTML terminado y lo enviamos a `script.php`, que recibe los datos mediante el método GET. La dirección de la solicitud de la página podría ser así:

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
> Por lo tanto, en lugar de `isset($_GET['x']) && isset($_GET['y'])`, puede especificar simplemente:
>
> `isset($_GET['x'], $_GET['y'])`.

Tratamiento de los datos recibidos por el método POST
--------------------------------------

Si los datos se reciben por el método POST, la URL del script a procesar siempre tendrá este aspecto:

`https://________.com/script.php`

Y nunca de otra manera. Simplemente no. Los datos están ocultos en la petición HTTP y no podemos verlos.

> El método POST oculto es necesario para enviar nombres de usuario y contraseñas por razones de seguridad.
>
> **Seguridad:** Si trabajas con contraseñas en tu sitio, el formulario de inicio de sesión y registro debe estar alojado en HTTPS y debes hacer hash de las contraseñas de forma adecuada (por ejemplo, con BCrypt).

Gestión de solicitudes ajax
------------------------------

En algunos casos, al procesar las peticiones ajax, puede no ser fácil recuperar los datos. Esto se debe a que las bibliotecas ajax suelen enviar los datos como `json payload`, mientras que la variable superglobal `$_POST` sólo contiene datos de formulario.

Se puede seguir accediendo a los datos, he descrito los detalles en el artículo <a href="/ajax-post">Manejo de peticiones ajax POST</a>.
