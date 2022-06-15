Variables superglobales
=======================

> id: bc2107ee-6551-4559-8c02-9cecdf98d33b
> slug:
> 	cs: superglobalni-promenna
> 	es: variables-superglobales
> 
> publicationDate: '2019-11-01 09:29:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '792e217f1331f38ed0719d7abeb05e67'

Las variables superglobales se utilizan para pasar el estado global de la aplicación y la comunicación HTTP.

La principal ventaja de estas variables es que siempre y en todo momento están disponibles. En la práctica, se trata de matrices de valores en las que accedemos a información específica por índice. En diferentes contextos, la disponibilidad de las llaves puede variar (se explica a continuación).

Tipos de variables superglobales
--------------------------------

Todos los superglobales en PHP son arrays y se denotan con un signo de dólar seguido de un guión bajo (excepto `$GLOBALS`) y caracteres en mayúscula.

En `PHP 7` hay en particular lo siguiente:

| Variable Descripción
|-------------|-------|
| `$_GET` | Parámetros de la URL <a href="/methods-odesilani-dat">enviados por el método GET</a>
| `$_POST` | Datos del formulario <a href="/methods-odesilani-dat">enviados por POST</a>. Ten en cuenta que <a href="/ajax-post">puede comportarse de forma diferente en ajax</a>.
| `$_REQUEST` | Datos del formulario enviados por cualquier método (`$_GET`, `$_POST` y `$_REQUEST`).
| `$_FILES` | Información técnica sobre los archivos cargados actualmente, por ejemplo mediante la construcción `<input type="file">`.
| `$_SERVER` | <a href="/info">Configuración del servidor web</a>, dirección IP, configuración... varía en función del entorno (al llamar a un script PHP desde Terminal contendrá valores diferentes y por ejemplo faltará información sobre la petición actual).
| `$_COOKIE` | Configurado <a href="/cookies">cookies</a>.
| `$_SESSION` | Datos de la sesión (<a href="/sessions">sesión</a>), si existe y se ha establecido en el pasado.
**¡Atención, no contiene un guión bajo en el nombre!** Esta es la llamada <a href="global-variable">variable-global</a> y una notación alternativa para la palabra clave `global`. Si tienes una variable global `$variable` en tu aplicación, también puedes acceder a ella con la construcción `$GLOBALS["variable"]`. Sin embargo, el uso de variables globales es una solución mala e impura por diseño, así que mejor no lo hagas.
| `$_ENV` | Información sobre el entorno actual donde se está ejecutando PHP.

Es fácil hacer una lista de todos los valores existentes:

```php
foreach ($_SERVER as $key => $value {
	echo $key . ':' . $value . '<br>';
}
```

> Nota: No todos los índices deben existir siempre (por ejemplo, si el script ejecuta cron en modo CLI, el índice con la URL de la página o la dirección IP de la solicitud no existirá).

Acceso a las variables
-------------------

Recomiendo que todas las variables globales (excepto `$_SESSION`) sean de sólo lectura. Esto se debe a que contienen datos globales de la aplicación y otro código puede tenerlos en cuenta (por ejemplo, otra biblioteca instalada).

Otra desventaja del estado global es que no siempre se puede confiar en los valores exactos, aunque existan, por lo que siempre hay que comprobar sus claves con la construcción `isset()`.

Para guardar una nueva cookie, utilice `setcookie()` y no inserte el valor directamente. Esto se debe a que es de sólo lectura.

Lecciones aprendidas
-------

Nunca confíes ciegamente en los valores de las variables superglobales.

El usuario puede utilizar la URL y las cabeceras enviadas para influir en el establecimiento de los valores. Todas las entradas deben ser siempre cuidadosamente validadas.

Registrar globales - el problema con la versión antigua de PHP
------------------------------------------

En la versión antigua de PHP (hasta `5.4.0`), había una directiva especial `register-globals` (configurable en `php.ini`) que hacía que todos los parámetros pasados en una URL se registraran automáticamente como variables.

Por ejemplo:

Un usuario llegó a la URL: `https://example.com/script.php?var=24`

Y PHP creó automáticamente una variable `$var` con el valor `24` dentro del script.

Así que funcionó de forma clásica:

```php
echo $var;
```

Por lo tanto, cualquiera podría introducir cualquier variable en el script y cambiar su contenido. Obviamente, la seguridad no siempre fue una prioridad. La verdad es que no.

Otras fuentes
------------

Para una descripción más detallada, consulte el <a href="https://www.php.net/manual/en/language.variables.superglobals.php">manual oficial</a>.
