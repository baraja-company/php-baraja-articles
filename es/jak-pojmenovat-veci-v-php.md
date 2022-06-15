Cómo nombrar variables, funciones, métodos y clases
===================================================

> id: f70ac75d-422f-4696-88d5-9e1b843e060a
> slug:
> 	cs: jak-pojmenovat-veci-v-php
> 	es: como-nombrar-variables-funciones-metodos-y-clases
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: '10510114a0160c0826c9ee1f54c95b03'

Para mantener el código en orden, es importante elegir reglas claras sobre cómo derivar los nombres. Esta página sirve como resumen de los enfoques relativamente populares utilizados por un gran número de programadores, entre los que me incluyo y la gente con la que trabajo.

Si trabajas en un equipo de desarrollo, no dudes en utilizar sus reglas, pero para el desarrollo general es igual de beneficioso establecer algunos buenos hábitos.

Debido a que el concepto de toda la sintaxis de PHP es realmente amplio, he dividido la guía aquí en muchas categorías que son altamente especializadas.

Si buscas una solución rápida, te recomiendo estudiar <a href="https://www.php-fig.org/psr/psr-4/">la norma PSR-4</a>.

Insertar scripts, enlazar con HTML, cargar archivos
---------------------------------------------------

Cada script debe comenzar con la etiqueta `<?php`.

Si no hay **nada de HTML** al final del archivo, no debe terminarse (para evitar un carácter blanco al final de la página).

La carga de otros archivos debe seguir las siguientes reglas:

- Si el archivo no es crítico y se puede prescindir de él al renderizar la página (por ejemplo, información adicional en el pie de página), debe cargarse mediante include: `include 'archivo.php';`
- Archivos críticos sólo a través de la construcción require: `require 'archivo.php';`
- Si estamos trabajando con objetos, usamos la construcción require para cargar el autoloader, que se encargará de cargar las otras clases por sí mismo (la mayoría de los frameworks implementan este comportamiento).


Si es al menos un poco posible, deberíamos separar la lógica de recuperación de datos del renderizado a HTML, es decir, utilizar el modelo MVC (modelo - vista - controlador).

Así que primero preparamos los datos en, por ejemplo, Presenter:

`Presentador.php`

```php
$cisla = [1, 2, 3];

$data = [];
$data['números'] = $cisla; // pasar los datos a la plantilla
include 'renderCisel.php'; // cargar plantilla
```

Y luego dibujarlo en la plantilla:

`renderCisel.php`

```html
<table>
<?php
    foreach ($data['cisla'] as $cislo) {
        echo '<tr><td>' . $cislo . '</td></tr>';
    }
?>
</table>
```

Este enfoque es utilizado por la mayoría de los frameworks y es útil para aumentar la claridad del código. En la última parte del proceso de desarrollo, este enfoque debería ser utilizado por todos los programadores experimentados que quieran desarrollar aplicaciones claramente estructuradas (para las aplicaciones grandes este enfoque es imprescindible).

Nombres de las variables
----------------

Si una variable contiene una matriz de valores u otros objetos, debe nombrarse en plural:

```php
$numbers = [1, 2, 3];
```

Porque entonces podemos simplemente iterar los valores por un solo número:

```php
foreach ($numbers as $number) {
    // procesamiento de números
}
```

Un nombre compuesto por varias palabras se combina en una sola palabra larga con la sintaxis cameCase, es decir, la primera palabra comienza con una letra minúscula y cada palabra posterior con una letra mayúscula:

```php
$promenna = '¡Oye! ¡Oye!';
$seznamUzivatelu = [
   'Jan Barášek',
   'Barack Obama',
   'Steve Jobs',
   'Stephen Wolfram',
];
$maxFilesInDirectory = 12;
$nameOfPhpScript = 'index.php'; // Se ha reducido el tamaño de la abreviatura PHP
```

Nombres de funciones y métodos
--------------------

Las funciones y los métodos siempre deben dejar claro en su nombre lo que hacen. A menudo, los parámetros de entrada y el valor de retorno esperados también pueden incluirse en el nombre.

Intenta adivinar qué hacen las siguientes funciones y cuál es su valor de retorno:

```php
getUserById($id);
saveErrorToLog($message);
createDefaultDirectory($path);
setAuthors(['Jan Barášek', 'Chuck Norris']);
getCurrentTime();
```

Todo el truco está en la primera palabra del nombre, que deja claro qué método utilizará la función. Se suele seguir la siguiente convención:

- `get` - recupera datos como una matriz u objeto, los parámetros de entrada especifican la entidad que se busca
- `save` - guardar en un archivo o base de datos
- `create` - crear una entidad (por ejemplo, crear una instancia de un objeto)
- `set` - guardar datos en una variable predefinida (dentro de una función)

Nombres de las clases
----------

Una clase es una entidad grande que contiene un gran número de propiedades y métodos, por lo que también debe comenzar con una letra mayúscula. Una clase también debe llevar una sola entidad (y describir sus propiedades), por lo que debe ser nombrada con un nombre singular. Si necesitamos trabajar con múltiples entidades, podemos simplemente almacenar cada instancia en un array.

Ejemplo:

```php
class User
{
    public string $username;

    public string $password;

    public string $role;
}

class Users
{
    /** @var Usuario[] */
    public array $users;

    public function addUser(User $user): void
    {
        $this->users = array_push($this->users, $user);
    }
}
```

La clase **Usuario** se especializa en información sobre un solo usuario específico. Si queremos trabajar con múltiples usuarios, creamos otra clase (envelope) que lleva un array de instancias de una entidad específica.

Las fábricas también pueden ser útiles para esto, ya que nos permiten crear fácilmente objetos similares y reciclar las instancias originales, lo que resulta en un código más claro mientras se ahorran recursos del sistema.

Espacio de nombres - Espacios de nombres
---------------------------

Aunque Namespace es independiente del directorio físico en el que está disponible el script, es una buena práctica respetar al menos parcialmente la disposición del proyecto (lo que conduce a un mejor sistema de creación de nuevos nombres que es más inequívoco de esta manera).

Personalmente, nombro los espacios de nombres según el subdirectorio común para las clases de ese tipo.

Ejemplos:

```php
App\Presenters; // Estos son los nombres de todos los presentadores
App\Model;      // Este es el nombre del modelo general
App\Model\Math; // Este es el nombre del modelo que trabaja con las matemáticas
```

Para una correcta autocarga de las clases, es conveniente seguir la norma <a href="https://jakpsatphp.cz/PSR4/">PSR-4</a>.

Convenciones personalizadas (por ejemplo, en un equipo corporativo)
-----------------------------------------

¿Y cómo llamas tú a los tuyos? Agradecería consejos para mejorar este artículo.

En general, sin embargo, las convenciones personalizadas dentro de un equipo no tienen mucho sentido, porque hace que el código sea más difícil de portar a otros frameworks y cuando se contrata a un nuevo compañero, éste tiene que aprender las convenciones actuales. Por lo tanto, es mejor seguir la norma `PSR-4`.
