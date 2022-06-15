Sesiones - cookies del servidor en PHP
======================================

> id: ab782ff0-d5ba-4115-9ebb-ab37978a6527
> slug:
> 	cs: sessions
> 	es: sesiones-cookies-del-servidor-en-php
> 
> publicationDate: '2019-11-06 17:06:18'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '3c84a09e5d180f6a47a58bea70b7ab21'

A menudo necesitamos almacenar más información en <a href="/cookies">cookies</a>, pero el límite máximo para las cookies es de 4 kB, que no es mucho. Sessions resuelve este problema almacenando los datos en el servidor web, y sólo almacena un breve identificador en el navegador del cliente para saber qué datos pertenecen a cada cliente.

Iniciar una sesión
---------------------

Antes de realizar cualquier trabajo con las sesiones, debemos iniciarlas. Esto se hace llamando a la función `session_start()` justo al principio del script:

```php
session_start();
```

> Advertencia fuerte: ¡no se debe ejecutar ninguna salida a código HTML antes de llamar a la función `session_start()`!

Seguridad de la sesión
-------------------

El contenido de la sesión se almacena en el servidor y sólo se envía el identificador al navegador del cliente, por lo que el usuario no tiene forma de saber qué se almacena en la sesión. La única forma en que el script puede afectar al usuario es borrando el identificador (con lo que el script generará uno nuevo).

Recuperación de datos de una sesión
----------------------

Todas las sesiones se almacenan en la variable superglobal `$_SESSION` y pueden ser recorridas como un array.

Por ejemplo, el nombre del usuario que ha iniciado la sesión puede obtenerse escribiendo

```php
echo $_SESSION['usuario'];
```

Nota: La sesión puede no existir siempre (por ejemplo, si es un usuario nuevo). Por lo tanto, siempre debemos comprobar la existencia antes de cualquier listado y ofrecer un mensaje de error alternativo si es necesario.

```php
if (isset($_SESSION['usuario']) && $_SESSION['usuario']) {
    echo 'Usuario registrado:' . $_SESSION['usuario'];
} else {
    echo 'No se ha registrado nadie.';
}
```

Guardar datos en la sesión
----------------------

El guardado se realiza como un simple guardado de datos en una variable:

```php
$_SESSION['usuario'] = 'Honzik';
```

El servidor web se encargará de la provisión técnica del correcto almacenamiento en el servidor y del envío del identificador al usuario.

Eliminación de sesiones
----------------

Los valores individuales se pueden borrar por separado según la clave:

```php
unset($_SESSION['usuario']);
```

O bien todas las sesiones disponibles:

```php
unset($_SESSION);
```

> Nota: La eliminación de una sesión específica no vacía el valor de la clave, sino que la elimina por completo. Por lo tanto, se lanzará una advertencia de error cuando se intente leer una clave inexistente. La existencia de una clave siempre se puede verificar fácilmente con la función `isset()`.

Validez máxima de la sesión
---------------------------------

Cada sesión guardada tiene un límite de tiempo de almacenamiento en el servidor. PHP contiene directamente un script cron que borra periódicamente las sesiones antiguas.

El valor por defecto suele ser `1440 segundos`, que son `24 minutos`.

El aumento del valor debe hacerse en 2 lugares:

- En <a href="/info">`php.ini` se establece la longitud máxima de validez que mantendrá el servidor</a>. El valor lo establece la directiva `session.gc_maxlifetime`,
- En el lado del script PHP, es necesario especificar la validez actual solicitada.

Uso en PHP:

```php
// el servidor mantendrá ahora la sesión hasta 3600 segundos = 1 hora
ini_set('session.gc_maxlifetime', '3600');

// todos los clientes (navegadores) serán
// sesión enviada con una validez de exactamente 3600 segundos
session_set_cookie_params(3600);

session_start(); // ¡podemos empezar la sesión!
```
