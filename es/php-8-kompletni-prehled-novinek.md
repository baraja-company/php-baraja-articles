PHP 8 está fuera - visión completa
==================================

> id: '8b6ce751-195f-41d2-82c6-1af4be3e86b5'
> slug:
> 	cs: php-8-kompletni-prehled-novinek
> 	es: php-8-esta-fuera-vision-completa
> 
> publicationDate: '2020-11-26 11:53:54'
> mainCategoryId: '17545205-215b-4962-b910-0d67ad1e933a'
> sourceContentHash: f657145ac1d2a1109fcc2a1ff3b6e6cf

Hoy, 26 de noviembre de 2020, la nueva versión principal de PHP 8 fue lanzada después de varios años, e incluye un conjunto de nuevas características audaces. Esta es una de las mayores actualizaciones en mucho tiempo y merece un artículo especial.

En este artículo, resumiremos las principales novedades y las diferencias de sintaxis y opciones respecto a la versión anterior. La mayoría de las nuevas funciones son compatibles con las anteriores y aportan mejoras de comportamiento que te gustarán.

> **Información importante:** PHP 8 está ahora en una fase de `congelación de características`, lo que significa que ya no se pueden añadir nuevos comportamientos y sólo se corrigen errores. De este modo, podrá contar con la compatibilidad y depurar completamente sus aplicaciones.

Tipos de unión
----------

PHP en general ha ido cambiando en los últimos años de un lenguaje puramente dinámico donde cualquier variable podía contener cualquier cosa a una forma estricta donde sabemos de antemano qué tipo de datos habrá en cada variable, parámetro, argumento o propiedad. El uso de [data-types](/datove-types) sigue siendo opcional, pero yo recomiendo el uso de la tipificación fuerte y la utilizo yo mismo en todos los proyectos.


Los tipos de unión expresan una colección de tipos múltiples, aceptando cualquier argumento o propiedad en ellos.

Por ejemplo:

```php
function validatePsc(string|int $psc): bool
{
	// aplicación
}
```

La función `validatePsc()` en la variable `$psc` acepta los tipos de datos `string` (cadena) e `int` (entero).

En la versión anterior de PHP 7.4, esta notación no era posible y normalmente se evitaba con [comment](/document-comment):

```php
/**
 * @param string|int $psc
 */
function validatePsc($psc): bool
{
	// aplicación
}
```

Sin embargo, este comentario de anotación es ignorado por PHP (es un comentario, después de todo) y tuvimos que realizar una comprobación adicional con una herramienta externa como PhpStan, que muchos desarrolladores ignoraron. Ahora la comprobación se realiza directamente en tiempo de ejecución (cuando la aplicación se está ejecutando) y no se puede eludir.

Sin embargo, PHP ha conocido un cierto tipo de unión desde la versión 7, cuando era posible decir que el tipo principal también podía ser `nullable`, es decir, que aceptaba el tipo de datos principal más el valor `null`.

Esto se escribió de dos maneras, cada una con un significado diferente:

```php
function setPhone(?string $phone): void
{
	// aplicación
}

// o

function setPhone(string $phone = null): void
{
	// aplicación
}

// o combinación

function setPhone(?string $phone = null): void
{
	// aplicación
}
```

Todas las entradas dicen que el teléfono `int` (entero) es una `cadena` o `null`.

- La primera notación requiere siempre pasar el valor
- La segunda notación no requiere que se pase ningún valor; si no se pasa nada, el valor por defecto es `null` (es un argumento opcional)
- La tercera entrada es una combinación de las opciones y se comporta como la segunda entrada

Al utilizar tipos de unión, ya no podremos utilizar una notación con un signo de interrogación y deberemos definir estrictamente el tipo de dato `null`, por ejemplo:

```php
function setPhone(string|int|null $phone = null): void
{
	// aplicación
}
```

El número de teléfono debe ser ahora `string`, `int` o `null`.

Los tipos de unión siguen teniendo una serie de usos, que los desarrolladores avanzados leerán en la documentación o en la implementación de bibliotecas específicas.


JIT: procesamiento más rápido de los scripts
----------------------------------

El compilador JIT (just in time) aporta una mejora significativa en el rendimiento de la complicación de los scripts (análisis sintáctico y comprensión). Sin embargo, este comportamiento puede variar en el contexto de las solicitudes web.

Ahora puede ver si tiene activado el JIT en la barra de Tracy dentro del marco de trabajo de Nette, y ver [artículo separado](https://stitcher.io/blog/php-jit) para más detalles.

Lo que se puede decir sobre la compilación en general es que PHP trata de procesar el código por adelantado para que cuando procese una petición particular, no tenga que ir a través del archivo de script físico, parsearlo e interpretarlo. En el pasado, esto se gestionaba a través de la extensión OPCache (que los servidores y hosts tienen disponible por defecto) y mejoraba la velocidad de procesamiento a la mitad aproximadamente.

Como regla general, si tienes una aplicación lenta, siempre es mejor elegir un algoritmo adecuado para manejar una tarea particular que hacer micro-optimizaciones en el código. Por lo general, los grandes retrasos se deben a la espera de la base de datos y sus lentas consultas, al almacenamiento de las sesiones, a la espera de que el espacio del disco duro esté disponible y a otras operaciones de hardware.

Operador nulo (encadenamiento opcional)
-------------------------------------

Muy a menudo, en una aplicación real, es necesario verificar la existencia de un valor de retorno (que no sea `null`) de un método y luego llamar condicionalmente a otro. Los [operadores ternarios](/operador ternario) son estupendos para esto, pero sólo funcionan con una condición y no se pueden anidar. El operador nullsafe permite el anidamiento de forma nativa.

> **TIP:** Prácticamente el mismo comportamiento ya es soportado por el sistema de plantillas Latte, pero anula este tipo de sintaxis en el código nativo de PHP, por lo que se puede utilizar el operador nullsafe en versiones anteriores de PHP (a partir de PHP 7). ¡Felicidades a David por esta modificación!

Facilita su uso:

```php
$orderId = $order?->getId();
```

La variable `$orderId` contiene el valor devuelto por el método `getId()`, o `null` si la variable `$order` contiene el valor `null` y no se ha podido llamar al método `getId()`.

Este tipo de problema se evitó en PHP 7 mediante la siguiente sintaxis a través del operador ternario:

```php
$orderId = isset($order) ? $order->getId() : null;
```

Posiblemente una condición:

```php
if (isset($order)) {
	$orderId = $order->getId();
} else {
	$orderId = null;
}
```

La entrada puede escribirse más adelante en la convocatoria. Tomé la muestra de [documentación de Latte](https://latte.nette.org/cs/syntax#toc-volitelne-retezeni-s-nullsafe-operatorem), que la describe perfectamente:

```php
$orderName = $order->item?->name;

// igual que:

$orderName = isset($order->item) ? $order->item->name : null;
```

El uso típico es cuando se listan estructuras más complejas en una plantilla, por ejemplo en Latte se ve así (muestra tomada de la documentación):

```php
{$user?->address?->street}
// significa approx ($user !== null) && ($user->address !== null) ? $user->dress->street : null

{$items[2]?->count}
// replace approx ($items[2] !== null) ? $elementos[2]->cuenta : null

{$user->getIdentity()?->name}
// replace approx $user->getIdentity() !== null ? $user->getIdentity()->nombre : null
```

En código real puede verse así, por ejemplo, que queremos averiguar el país del cliente leyendo su perfil (y tienes los datos en la base de datos almacenados bien a través de sesiones, como se supone), entonces en el antiguo PHP se veía así:

```php
$country =  null;
if ($session !== null) {
    $user = $session->user;
    if ($user !== null) {
        $address = $user->getAddress();
        if ($address !== null) {
            $country = $address->country;
        }
    }
}
```

Ahora se puede acortar a una sola línea:

```php
$country = $session?->user?->getAddress()?->country;
```

El uso del operador nullsafe también evita varios errores que no podrían ser fácilmente detectados por un desarrollador inexperto en PHP 7.

Por ejemplo, esta entrada generará un error fatal:

```php
var_dump($invoice->getDate()->format('Y-m-d') ?? null);

// return: fatal error: uncaught Error: llamada a una función miembro format() en null
```

La sintaxis correcta es la siguiente:

```php
var_dump($invoice->getDate()?->format('Y-m-d'));

// devuelve: null
```

Argumentos con nombre
---------------------

En el viejo PHP, las llamadas a funciones con argumentos debían escribirse pasando los argumentos en el orden exacto definido por la función de destino. No hay nada malo en ello, sin embargo, cuando se utilizan varios parámetros con valores similares, podría causar una peor legibilidad. O si quisiéramos pasar hasta el enésimo parámetro de la orden, todos los parámetros opcionales tendrían que ser pasados antes, lo que podría tener un efecto negativo en la legibilidad y la compatibilidad hacia adelante.

Imagina, por ejemplo, la función `setCookie()` de Nette, que tiene muchos argumentos:

```php
public function setCookie(
	string $name,
	string $value,
	$time,
	string $path = null,
	string $domain = null,
	bool $secure = null,
	bool $httpOnly = null,
	string $sameSite = null
)
```

Los tres primeros argumentos (`$nombre`, `$valor` y `$tiempo`) son obligatorios, pero si queríamos pasar el argumento `$httpOnly`, teníamos que pasar todos los anteriores y calcular el orden correctamente:

```php
$http->setCookie(
	'miCookie',
	'A David le gustan los caballos',
	'ahora',
	null, // ruta de acceso
	null, // dominio
	null, // seguro
	true
);
```

Lo que simplemente no quieres hacer si no tienes que hacerlo.

La escritura elegante entonces parece:

```php
$http->setCookie(
    name: 'miCookie',
    value: 'A David le gustan los caballos',
    time: 'ahora',
    httpOnly: true
);
```

Este tipo de sintaxis requiere que los nombres de los argumentos de la función de destino no cambien nunca, porque seguirán estando tipificados cuando se les llame. Al menos los desarrolladores podrán nombrarlos mejor.

Si queremos utilizar sólo uno de los argumentos, la sintaxis puede combinarse y condensarse en una sola línea:

```php
$http->setCookie('miCookie', 'A David le gustan los caballos', 'ahora', httpOnly: true);
```

Los primeros 3 argumentos se pasan de la manera original, luego se pasa el argumento opcional `httpOnly` (porque se nombra).

Atributos
---------

La mayoría de los principales lenguajes, como Java o C#, ya incluyen de forma nativa las llamadas anotaciones, que es una sintaxis nativa del lenguaje que permite añadir metainformación a otras construcciones del lenguaje.

En PHP, este tipo de sintaxis ha estado ausente durante mucho tiempo, y se ha eludido mediante el uso de comentarios DOC, que es un comentario clásico sobre un método, excepto que tiene dos asteriscos `/**`.

Estos comentarios son ignorados durante el procesamiento de los scripts y debe añadirse una lógica de usuario especial para analizarlos e interpretarlos en tiempo de ejecución a través de la reflexión. Probablemente puedes entender el impacto en el rendimiento que esto puede tener, además la sintaxis de los comentarios no puede ser requerida y es muy difícil de comprobar en tiempo de compilación (cuando el script es procesado antes de ser ejecutado), y de nuevo tienes que usar herramientas adicionales fuera del conjunto de herramientas normales de PHP para hacerlo.

Para preservar la compatibilidad con versiones anteriores, PHP proporciona atributos con una sintaxis similar a la notación de comentario alternativa, que no rompe la ejecución del script en PHP heredado.

La notación original (utilizada, por ejemplo, para las dependencias Inject en Nette Presenter):

```php
final class HomepagePresenter extends BasePresenter
{
	/** @inject */
	public EntityManager $entityManager;
}
```

Ahora puedes eliminar el comentario y utilizar el atributo nativo:

```php
use App\Attributes\Inject;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Es muy importante que el atributo ya no sea sólo un trozo de cadena en un comentario, sino una clase física que es código PHP válido.

Esto es genial, porque ahora puedes validar con seguridad las entradas de un atributo, y el uso de un atributo se convierte realmente en una llamada a su constructor donde se puede utilizar otra lógica. Estoy deseando ver esto soportado de forma nativa por Doctrine, que utiliza anotaciones para todo.

La implementación del atributo en sí podría ser algo así:

```php
#[Attribute]
class Inject
{
    public string $value;

    public function __construct(string $value)
    {
        $this->value = $value;
    }
}
```

La lógica estricta se puede utilizar dentro de los atributos de nuevo, como la comprobación de los tipos de datos de los argumentos, los tipos de unión y otras características del lenguaje.

Expresión de coincidencia
----------------

La nueva construcción del lenguaje `match()` es una mejora modernizada del viejo `switch()` (que intento no utilizar), y aporta una serie de características interesantes (que harán que empiece a utilizarlo de nuevo).

Por ejemplo, queremos modificar el valor de una variable en función de la entrada:

```php
$pozdrav = match(bool $formal) {
    true => 'Hola',
    false => 'Hola',
};
```

Una nueva característica importante de la sintaxis es que no tenemos que usar `break` (como el antiguo `switch`) y la sintaxis es en general mucho más económica.

Al mismo tiempo, podemos validar varias entradas a la vez dentro de una condición (separadas por una coma) y posiblemente devolver un valor por defecto (cuando no se satisface ninguna).

Esto es muy útil cuando se reescribe el código de condición HTTP a un mensaje de error, por ejemplo (definitivamente lo apreciará cuando maneje códigos de excepción):

```php
$message = match ($statusCode) {
    200, 300 => null,
    400 => 'no se encuentra',
    500 => 'error del servidor',
    default => 'código de estado desconocido',
};
```

La comparación de valores se realiza estrictamente mediante el operador `===` (switch sólo utiliza `==`), lo que demuestra de nuevo que PHP sigue el camino del diseño estricto. Por lo tanto, la entrada `'200'` (una cadena que contiene un número) no será aceptada en el caso anterior.

Si no se especifica un valor para `default` y no hay ninguna coincidencia, se lanza un `UnhandledMatchError`.

La nueva sintaxis también permite utilizar una expresión o una llamada a una función para realizar una coincidencia (se comporta como una condición). En caso de error, podemos lanzar una excepción (ya que el token `throw` es ahora una expresión y puede ser utilizado de esta manera):

```php
$message = match ($statusCode) {
    200 => null,
    $this->checkServerError($statusCode) => throw new ServerError(),
    default => 'código de estado desconocido',
};
```

Propagación de propiedades en el constructor
-------------------------------------------------

Esto es sólo un azúcar sintáctico que será útil para la definición rápida y fácil de una entidad y sus propiedades directamente en el constructor.


Por ejemplo, la entidad original:

```php
final class User
{
    public string $name;

    public function __construct(
        string $name,
    ) {
        $this->name = $name;
    }
}
```

Sólo se puede acortar a:

```php
final class User
{
    public function __construct(
        public string $name
    ) {}
}
```

La propiedad `$name` se valida con el tipo de datos `string` y su valor se puede leer directamente de la instancia porque es una propiedad pública. Si usas un SmartObject extra en Nette (lo cual no recomiendo para PHP 8), también puedes acceder a las propiedades privadas llamando primero a su método getter, y esta sintaxis simplifica las cosas de nuevo.

Tipo de retorno estático
--------

Antes podíamos utilizar el tipo de dato `self` como valor de retorno de un método, pero éste devuelve una instancia de la propia clase donde está definido. El tipo de datos `static` generalmente puede hacer esto incluso en el caso de la herencia, y devolverá el tipo de datos de la clase desde la que se ejecutó la instancia, no su ancestro.

Por ejemplo:

```php
class BaseEndpoint
{
    public function getInstance(): static
    {
        return new static();
    }
}
```

Tipo de datos mixtos
------------------

El tipo `mixto` puede utilizarse ahora como argumento de una función o método. Esto significa que el método siempre debe aceptar alguna entrada (y por lo tanto es un argumento obligatorio).

Si puedes al menos un poco, utiliza siempre un tipo de dato directo, o al menos una unión. Mixed sólo es útil si la función acepta realmente cualquier cosa. En la práctica, el uso es útil, por ejemplo, para varias funciones de volcado que aceptan una entrada arbitraria y deben ser capaces de mostrarla.

El tipo `mixed` acepta los siguientes tipos: `string`, `int`, `float`, `null`, `bool`, `array`, `callable`, `object`, `resource`.

David utilizará entonces el tipo mixto para su función:

```php
function bdump(mixed $var): mixed
{
	Tracy\Debugger::barDump($var);
	return $var;
}
```

Lanzamiento de fichas como expresión
------------------------

El token `throw` se ha convertido ahora en una expresión, esto significa en la práctica que se puede lanzar una excepción cuando la función lambda `fn()` se trunca, o cuando se comprueba un operador ternario:

```php
$error = fn () => throw new \InvalidArgumentException('Esto siempre arroja un error.');

$userName = $user['nombre'] ?? throw new \LogicException('El usuario debe tener un nombre.');
```

La función str_contains()
-----------------------

PHP finalmente incluye una función nativa para comprobar que la cadena por defecto contiene una subcadena.

Por ejemplo:

```php
if (str_contains('A Honzik le gustan los gatos.', 'gatos')) {
	echo 'La función se encarga de los gatos.';
}
```

En el pasado, la aparición de una subcadena se verificaba mediante la función [strpos](/strpos-function):

```php
if (strpos('A Honzik le gustan los gatos.', 'gatos') !== false) {
	echo 'La función se encarga de los gatos.';
}
```

Funciones str_starts_with() y str_ends_with()
----------------------------------------------

Un par de nuevas funciones para comprobar si una cadena empieza o termina con una subcadena:

```php
str_starts_with('A Honzik le gustan los gatos.', 'Honzik'); // verdadero

str_ends_with('A Honzik le gustan los gatos.', 'gatos.'); // verdadero
```

Función get_debug_type()
-------------------------

Mejora la salida de la función [gettype](/función-gettype) existente, que devolvía sólo el tipo genérico de la variable pasada. La función se utiliza, por ejemplo, al lanzar una excepción, cuando obtenemos una entrada no válida y queremos decirle al usuario lo que realmente pasó.

Cuando llamamos a la función `gettype()` con una variable que contiene una instancia de la clase `\App\User`, la función devuelve `object`, por lo que no sabemos de qué clase se trata. La nueva función `get_debug_type()` devuelve el nombre de la clase.

La función get_resource_id()
--------------------------

Esta función devuelve el identificador de un recurso externo de una variable.

Por ejemplo, la conexión a una base de datos MySql es manejada por PHP mediante el uso de un tipo de datos especial `resource`, ahora es posible averiguar qué ID se le ha asignado.

> **Nota histórica:**
>
> El tipo `resource` en PHP fue creado en un momento en el que todavía no se sabía cómo usar objetos, y tuvo que averiguar de alguna manera cómo pasar referencias a algo como un `tipo de datos`. En el futuro, es de esperar que se elimine el término "recurso" del lenguaje, por lo que es mejor no utilizar esta función.

La extensión ext-json está siempre disponible
-------------------------------------

En el pasado, PHP podía ser compilado sin soporte para json. Ahora, json siempre estará disponible, por lo que puedes eliminar la dependencia `ext-json` de tus archivos `composer.json` y saber siempre que se puede utilizar json.

Precedencia de la concatenación
-------------------------------------------------------

Imagina algo así:

```php
echo 'La suma total:' . $a + $b;
```

¿Se hace primero la suma de números o se añade primero la variable `$a` a la cadena y luego se añade toda la nueva cadena a `$b`?

Uno esperaría que la adición se hiciera primero, pero es una bonita suposición. En realidad, PHP hace algo así:

```php
echo ('La suma total:' . $a) + $b;
```

PHP 8 se comportará ahora de forma predecible:

```php
echo 'La suma total:' . ($a + $b);
```

Sin embargo, en general, siempre es mejor utilizar paréntesis para delimitar una expresión.

Ordenación estable
---------------

Antes de PHP 8, la ordenación de cadenas se realizaba utilizando el llamado algoritmo inestable, lo que significa que PHP no garantizaba que los elementos con el mismo valor (o equivalente) no se intercambiaran. La nueva versión cambia el comportamiento de todas las funciones de ordenación a estable, por lo que la ordenación se realiza siempre de forma determinista y siempre se obtiene la misma salida.

Esto resuelve, por ejemplo, los casos en los que clasificábamos las valoraciones de los usuarios por su relevancia, pero algunas valoraciones tenían la misma puntuación. Ahora aparecerán en el mismo orden cada vez que clasifiques y no saltarán continuamente.

Otras novedades
---------------

PHP tiene muchas otras novedades y mejoras menores. Por ejemplo, los errores se lanzarán de forma diferente (pero eso no nos molesta a los que escribimos código sin errores, ¿verdad?).

Siempre puedes ver la lista completa de cambios en la documentación oficial y en el post RFC.

Lo que echo de menos en el nuevo PHP
-----------------------

Me gustaría que PHP finalmente soportara tipos de array compuestos, por ejemplo cuando un método devuelve un array de identificadores todavía tenemos que especificar sólo `getIds(): array` y algo como `getIds(): int[]` sería mucho mejor. Tal vez lo veamos pronto y la comprobación de tipos fuertes sea completa.

Más recursos
------------

David Grudl dio una bonita charla sobre las novedades de Posobot. Recomiendo ver la grabación:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/ZYkwmcCUTCg" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Esto es para agradecer a David su conferencia, ya que he sacado alguna información de ella para este artículo. En particular, cosas sobre el paso de Nette a PHP 8 y otros consejos entre bastidores sobre PHP.
