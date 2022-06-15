Procesamiento de peticiones ajax POST en PHP
============================================

> id: c9c8fdb4-020d-4361-b425-4f4406a090ba
> slug:
> 	cs: ajax-post
> 	es: procesamiento-de-peticiones-ajax-post-en-php
> 
> publicationDate: '2019-11-01 09:56:02'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '89254da0b9b22b1926410b3ef7e511ec'

Mientras desarrollaba aplicaciones ajax Vue.js, después de años finalmente descubrí cómo usar ajax en PHP y cómo recibir datos por el método POST.

La variable superglobal `$_POST` sólo está disponible para los formularios
-------------------------------------------------------------

En PHP, la <a href="/superglobal-variable">variable superglobal `$_POST`</a> está comúnmente disponible para mantener los datos enviados desde un formulario.

Es **relativamente** fácil de usar.

En el lado de HTML, necesita crear un formulario:

```html
<form action="process.php" method="post">
    Jméno: <input type="text" name="username">
    <input type="submit" value="Odeslat">
</form>
```

Y luego en el archivo `process.php` se puede acceder a los valores como elementos del array:

```php
echo htmlspecialchars($_POST['nombre de usuario'] ?? '');
```

> **Atención:**
>
> Con este sencillo enfoque, todo el mundo podría pensar que los datos POSTed se definen automáticamente como índices de matriz en la variable `$_POST`. Pero esto no es cierto.

De hecho, la razón por la que los datos enviados desde un formulario utilizando el método POST se escriben en la variable `$_POST` es porque el navegador envía automáticamente la cabecera HTTP `'Content-Type': 'application/x-www-form-urlencoded'` cuando se envía el formulario HTML.

Sin una cabecera correctamente configurada, simplemente no se puede acceder a los valores y tenemos que utilizar una solución engañosa.

Envío de datos por ajax
-------------------

Al intentar enviar datos mediante ajax, tenemos que cambiar un poco el enfoque en el lado de PHP. Puedes <a href="https://www.facebook.com/groups/frontendisti/permalink/2372671669611010/">leer el debate en Facebook</a> para conocer los detalles.

En javascript, por ejemplo, puedes utilizar la librería <a href="https://github.com/axios/axios">axios</a> para enviar datos mediante ajax. Para utilizarlo fácilmente, basta con enlazar el javascript del servidor CDN y utilizarlo directamente:

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

<script>
    axios.post('/api/form-process', {
        username: 'Jméno uživatele'
    })
    .then(response => {
        // Zpracování odpovědi z API
        alert(response.data.message); // Vyhodí hlášku se zprávou
    });
</script>
```

En este caso sencillo, se llama a la URL `'/api/form-process'` mediante ajax y el método POST pasa el objeto `{ username: 'User name' }`. La propia biblioteca ya se encarga de la logística de pasar los datos automáticamente, por lo que se envían serializados como Json. En la jerga de los desarrolladores de frontend, esto se llama **json payload**.

Por el lado de PHP, esperaría usarlo como un formulario (después de todo, era un método POST):

```php
echo htmlspecialchars($_POST['nombre de usuario'] ?? '');
```

Sin embargo, en este caso el campo `$_POST` estará vacío y no se pasará ningún dato. La variable `$_POST` sólo se utiliza para los datos recuperados de los formularios (se puede saber por la cabecera HTTP que no tiramos).

Así que en este caso necesitamos obtener los datos directamente de la petición HTTP, para lo cual se utiliza la **solución inteligente**. Así, es fácil de usar:

```php
$data = json_decode(file_get_contents('php://entrada'), true);

echo htmlspecialchars($data['nombre de usuario'] ?? '');

header('Content-Type: application/json');
echo json_encode([
    'mensaje' => 'La enredadera del servidor',
]);
die;
```

Como ejemplo, también proporciono una respuesta sencilla en javascript. Lo importante es lanzar correctamente la cabecera HTTP `'Content-Type: application/json'` y salir del script una vez enviados todos los datos.

Imposición del uso de `$_POST`.
-------------------------

Si todavía quiere tratar los datos enviados directamente como un formulario, hay una manera de transferirlos. En este caso, hay que modificar la creación de la propia consulta ajax y pasar las cabeceras HTTP correctamente:

```js
axios.post(
    '/api/form-process',
    {
        username: 'Jméno uživatele'
    },
    {
        headers: {'Content-Type': 'application/x-www-form-urlencoded'}
    }
).then(response => {
    // Nějaké zpracování
});
```

En este caso, sin embargo, el procesamiento en el lado de PHP no será agradable, porque todavía tenemos que corregir los datos y convertirlos en un array.

Me las arreglé para llegar a este horror:

```php
if (\count($_POST) === 1
    && preg_match('/^\{.*\}$/', $post = array_keys($_POST)[0])
    && ($json = json_decode($post)) instanceof \stdClass
) {
    foreach ($json as $key => $value) {
        $_POST[$key] = $value;
        unset($_POST[$post]);
    }
}

echo htmlspecialchars($_POST['nombre de usuario'] ?? '');
```

Sin embargo, es mucho mejor quedarse con el primer enfoque y utilizar el método `'php://input'` para recuperar los datos.
