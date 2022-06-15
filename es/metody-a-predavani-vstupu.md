Métodos en la transferencia de EPI y de insumos
===============================================

> id: '843fbfb4-daf2-4c2e-9d94-28d494025b2e'
> slug:
> 	cs: metody-a-predavani-vstupu
> 	es: metodos-en-la-transferencia-de-epi-y-de-insumos
> 
> publicationDate: '2020-02-16 20:49:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3e1bca690ed70479ef9807a1f2a1f23

Los métodos representan el comportamiento de un objeto porque permiten trabajar con su estado interno, así como influir en los objetos entre sí.

Representación de los métodos en el mundo real
----------------------------------

Considere cualquier objeto del mundo real, por ejemplo, un gato. Un gato tiene ciertas propiedades (nombre, color, peso, ...) que describimos usando propiedades y luego también comportamiento (maullar, caminar, dormir, ...) y lo describimos usando métodos. Dado que hay muchos gatos en el mundo ("Y por eso ladro como un trueno, que haya un millón de gatos") es importante recordar que un método es algo general que se aplica a todos los objetos de un tipo determinado por igual.

Aplicación práctica de un método
-----------------------------

En términos de sintaxis del lenguaje, definamos una clase para un gato:

```php
class Cat
{
    public string $name;

    public string $sound = 'Miau';
}
```

Después de crear una instancia de un gato en particular, podemos simplemente enumerar, por ejemplo, el sonido:

```php
$cat = new Cat;

echo $cat->sound; // dice "Miau"
```

¿Pero qué pasa si queremos formatear el sonido de una manera determinada al escribirlo? Entonces entran en juego los métodos:

```php
class Cat
{
    public string $name;

    public string $sound = 'Miau';

    public function getFormattedSound(): string
    {
        return 'Estoy haciendo ruido "' . $this->sound . '¡"!';
    }
}
```

Ya debe conocer el principio de las funciones del pasado. Las clases permiten escribir una función directamente en su cuerpo con una accesibilidad definida (al igual que las propiedades) y se denominan métodos.

La variable `$this` se comporta un poco "mágicamente". Almacena la instancia actual del objeto en el que nos encontramos. Si queremos leer un valor de una propiedad o llamar a otro método dentro de un método, sólo tenemos que hacerlo sobre la variable `$this`.

El método se llama entonces desde dentro del objeto como una función clásica (con un paréntesis al final) para decir que no es una propiedad:

```php
$cat = new Cat;

echo $cat->sound; // dice "Miau"

echo $cat->getFormattedSound(); // imprime '¡Estoy haciendo un sonido "Miau"!'
```

Constructor - método llamado al crear una instancia
--------------------------------------------------

Al crear una instancia de objeto, a menudo necesitamos definir cómo establecer su estado básico y qué parámetros (datos de entrada) son obligatorios.

En OOP para resolver este problema existe un método público especial `__construct` que podemos implementar voluntariamente y que siempre y sólo se llama al crear una instancia.

Ejemplo práctico:

```php
class Cat
{
    public string $name;

    public string $sound;

    public function __construct(string $name, string $sound)
    {
        $this->name = $name;
        $this->sound = $sound;
    }

    public function getFormattedSound(): string
    {
        return 'Estoy haciendo ruido "' . $this->sound . '¡"!';
    }
}
```

Al definir el constructor, nos hemos asegurado de que al crear una instancia, siempre tenemos que pasar 2 parámetros obligatorios (`nombre` y `sonido`) y el objeto se establecerá directamente.

Como el constructor es un método clásico, puede hacer algo más dentro con los datos insertados. Por ejemplo, cambiar el formato de la cadena adecuadamente, pero eso lo trataremos más adelante.

Crear una instancia es entonces fácil de nuevo, sólo tenemos que pasar los parámetros iniciales (se escriben entre paréntesis al nombre de la clase donde se llama al nombre de la clase y se crea la instancia):

```php
$cat = new Cat('Minda', 'Vrr');

echo $cat->name; // Dice "Minda"
echo $cat->sound; // imprime "Vrr"

echo $cat->getFormattedSound(); // imprime '¡Estoy haciendo un sonido "Vrr"!'
```

Gettery y settery
-----------------

Algunos métodos se llaman `getters` y `setters`. Estos son métodos comunes, es sólo una convención para llamarlos así. Los métodos se utilizan para obtener e insertar datos en un objeto. La principal ventaja es que podemos validar los datos antes de insertarlos y posiblemente corregirlos o rechazarlos y lanzar un error.

Para cada propiedad que pueda ser recuperada (queremos habilitar la recuperación de datos), se acostumbra a crear un getter que comienza con la palabra `get`. Si la propiedad devuelve un booleano (`true` o `false`), es habitual nombrar el principio del nombre del método con la palabra `is`.

Ejemplo simplificado:

```php
class Cat
{
    public string $name;

    public function __construct(string $name)
    {
        $this->setName($name);
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function isEmpty(): bool
    {
        return $this->name === '';
    }

    public function setName(string $name): void
    {
        $this->name = trim($name);
    }
}
```

Al crear una instancia de gato, pasamos su nombre al constructor (que siempre es obligatorio). Sin embargo, como queremos compartir la lógica para insertar el nombre tanto para el constructor como para el setter (el método para insertar un nuevo nombre), llamamos al método `setName()` dentro del constructor para asegurarnos de que se inserta el nombre.

Dentro del setter, utilizamos la función <a href="/function-trim">trim()</a>, que elimina automáticamente los espacios en blanco de ambos lados de la cadena insertada.

Podemos utilizar el método `getName()` para la salida. El método `isEmpty()` se puede utilizar para comprobar si está vacío.

> **Ventajas prácticas:**
>
> Una gran ventaja de los getters y setters son los **tipos de datos garantizados**. Si un getter afirma que devuelve una `cadena`, podemos confiar en él y obtener siempre una cadena.
>
> Un setter en su implementación también dice aceptar una `cadena`, lo que significa para la aplicación que cualquier cosa que el usuario introduzca obtendrá su entrada convertida a una `cadena` (si utiliza un tipo compatible, como `integer`) o recibirá un error de que el tipo no puede ser convertido (como `array`).

El mayor valor añadido de los getters y setters se apreciará especialmente en el uso práctico.

El método mágico `__toString()`
-----------------------------

Dentro de una clase, se puede definir un método mágico `__toString()` que se llama automáticamente cuando se intenta escribir un objeto como una cadena. El método debe devolver siempre una cadena.

Ejemplo de aplicación:

```php
class Cat
{
    public string $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function __toString(): string
    {
        return 'Hola, soy' . $this->name . ';)';
    }
}
```

El uso es entonces el siguiente:

```php
$cat = new Cat('Minda');

echo $cat; // Dice: "Hola, soy Minda ;)"
```

Resumen
-------

Hemos mostrado cómo definir métodos y luego llamarlos dentro de una instancia de objeto.

La próxima vez, veremos el principio de <a href="/encapsulación">encapsulación</a>, que aprovecha al máximo las propiedades de los métodos.
