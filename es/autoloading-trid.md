Autocarga de clases en PHP
==========================

> id: f6cd5762-261f-4153-b27b-075dd8b5ed13
> slug:
> 	cs: autoloading-trid
> 	es: autocarga-de-clases-en-php
> 
> publicationDate: '2020-02-09 10:00:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '441a1d4107bb32e8cfe4dbb926c2decd'

Seguro que lo sabes, cuando programamos scripts PHP dividimos el código en muchos archivos y para tener todas las partes disponibles las cargamos con una serie de llamadas `include`, `require` o preferiblemente `require_once`, que garantiza la carga una sola vez.

En el código se ve así:

```php
require_once 'Router.php';
require_once 'Página.php';
require_once 'Paginator.php';
```

La principal desventaja de este enfoque es que el programador tiene que asegurarse constantemente de que todo está cargado. Pero si carga mucho, pierde rendimiento innecesariamente y llega al disco muchas veces. Así que la solución manual sólo tiene problemas.

Carga automática nativa
-------------------

Afortunadamente, hay soporte nativo en PHP para la llamada **Autocarga de Clases**, que es la lógica en el código que carga un archivo de clases sólo cuando se necesita por primera vez (normalmente cuando se crea una instancia por primera vez).

Una aplicación sencilla podría ser la siguiente:

```php
spl_autoload_register(function (string $className): void {
    include 'src/' . $className . '.php';
});

$obj  = new MyClass1();
$obj2 = new MyClass2();
```

Al crear una instancia de la clase `MyClass1`, la función `spl_autoload_register` lee el archivo `MyClass1.php` del directorio `src`. Esta implementación asume que cada clase está en un archivo separado llamado por el nombre de la clase o interfaz.

Casos más complejos de autocarga
-------------------------------

En una aplicación real, pueden surgir muchas situaciones desagradables que complican la carga automática, por ejemplo:

- La clase o interfaz no existe en absoluto
- El archivo ya ha sido cargado una vez
- Hay varias clases o interfaces en el mismo archivo
- La ruta de acceso a la clase o interfaz no coincide con el nombre
- La ubicación de la clase o interfaz cambia con el tiempo

Sin embargo, no necesitamos programar nuestra propia solución para todo esto, sino que podemos utilizar la solución existente una vez diseñada.

Si está utilizando <a href="https://getcomposer.org/doc/01-basic-usage.md">Composer</a>, probablemente también esté utilizando su autocarga nativa. Esto se debe a que cuando se instala cualquier paquete, Composer genera automáticamente un `mapa de clases`, que es un resumen de las clases y sus ubicaciones físicas.

Entonces, al principio del código (normalmente en `index.php`) sólo tienes que usar

```php
require __DIR__ . '/vendedor/autoload.php';
```

Sin embargo, el autoloading se genera sólo una vez cuando se llama al comando `composer dump`, por lo que es necesario regenerar el autoloading cada vez que la aplicación cambia.

RobotLoader: una solución elegante para el desarrollo
----------------------------------------

Para el desarrollo local, me gusta mucho el paquete `nette/robot-loader`, que recorre automáticamente la estructura de directorios y almacena en caché las ubicaciones de las clases. Así, si cargamos una clase, primero busca en la caché y sólo si no existe, reincide automáticamente en el proyecto. Por lo tanto, el programador no tiene que seguir la pista de dónde se encuentra cualquier archivo o clase y se limita a programar.

Instalación a través de Composer:

```txt
composer require nette/robot-loader
```

La explicación básica de la funcionalidad se describe en la propia <a href="https://doc.nette.org/cs/3.0/robotloader">documentación</a>:

> De manera similar a como el robot de Google rastrea e indexa las páginas web, RobotLoader rastrea todos los scripts de PHP y anota qué clases, interfaces y rasgos encontró en ellos. A continuación, almacena en caché los resultados de su investigación y los utiliza en la siguiente solicitud. Así que sólo hay que especificar qué directorios rastrear y dónde almacenar en caché.

De este modo, es extremadamente fácil de usar:

```php
$loader = new Nette\Loaders\RobotLoader;

// añadir directorios que RobotLoader debe indexar
$loader->addDirectory(__DIR__ . '/app');
$loader->addDirectory(__DIR__ . '/libs');

// establecer la caché en el disco en el directorio 'temp'
$loader->setTempDirectory(__DIR__ . '/temp');
$loader->register(); // iniciar RobotLoader
```

Establecer `$loader->setAutoRefresh(true o false)` determina si RobotLoader debe reindexar los archivos cuando encuentra una nueva clase. Esto debería estar desactivado en los servidores de producción.

Ya está, ahora nunca más tendrás que lidiar con la carga automática.

Solución combinada
------------------

Utilizo una solución combinada cuando desarrollo un proyecto real.

La forma en que funciona en la vida real es que cargo los paquetes instalados a través de Composer autoload (que es muy eficiente) y esto resuelve la carga de todas las clases en el directorio `vendor`.

El código para el proyecto específico se coloca en el directorio `app`, donde manejo la carga automática de algunas clases a través de RobotLoader. Lo importante es mantener siempre la aplicación concreta lo más pequeña posible y utilizar paquetes ya hechos en la medida de lo posible, eso favorece mucho la reutilización.

La estructura del proyecto es la siguiente:

```txt
/app
    Bootstrap.php <-- konfigurace
    /model
        UserForm.php <-- projektové třídy
        RegisterFactory.php
        ...
/vendor
    ... <-- knihovny
/www
    index.php <-- inicializace
```

Pruebas de carga automática
------------------------

A veces puede ocurrir que no todos los archivos se carguen siempre y que encuentres problemas.

Para la depuración, recomiendo la función <a href="/get-list-of-all-loaded-files">get_included_files()</a>.
