Comandos, palabras clave y funciones en PHP
===========================================

> id: '96a00928-4410-479d-ada0-612de21cdacd'
> slug:
> 	cs: prikazy-a-funkce
> 	es: comandos-palabras-clave-y-funciones-en-php
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '4534cc03d2ae66454b9f398139f232a4'

Cada script PHP consiste en comandos y funciones, juntos se llaman **construcciones**. Cuando se procesa el script, se rastrean estas expresiones lingüísticas (este proceso se llama **tokenización**) y en base a las *palabras clave* el intérprete decide cómo debe comportarse el procesador.

Comandos
--------------------------

Los comandos (<a href="https://www.php.net/manual/en/reserved.keywords.php">se llaman **palabras clave** en inglés</a>) son partes del lenguaje ya preprogramadas, están reservadas específicamente para su propósito, pueden ser utilizadas inmediatamente en cualquier situación (no requieren bibliotecas especiales adicionales ni instalación), y comprenden la base de todos los scripts.

Por ejemplo, <a href="/echo">extraer una cadena en código HTML</a>:

```php
echo 'Echo es un comando de lenguaje para listar el contenido';
```

En el caso de los comandos, es importante tener en cuenta que no devuelven ningún valor y, por lo tanto, no pueden utilizarse como una variable. Los comandos también pueden identificarse por el hecho de que no contienen paréntesis.

Ejemplos:

```php
echo '¡Hola, mundo!';
echo ('¡Hola, mundo!');
```

Si encierro el contenido del comando entre paréntesis, el comportamiento no cambiará y se verá como una **expresión**. Es lo mismo que si escribiéramos en matemáticas:

```php
5 + 3

// o:

(5 + 3)
```

Técnicamente hay una diferencia, pero en la práctica no cambia nada.

Funciones
--------------------------

Si sólo hubiera comandos, sería bastante aburrido. Las funciones son una colección de comandos múltiples.

A menudo queremos ejecutar el mismo código en varios lugares repetidamente. En ese caso, copiar constantemente sería demasiado trabajo y podría dar lugar a errores, por lo que envolvemos ese código en una función a la que damos un nombre y luego simplemente la llamamos por su nombre.

Cuando llamamos a la función, le pasamos los parámetros como variables, ejecuta el código y devuelve el resultado, que podemos utilizar posteriormente.

```php
$text = 'PHP es mi lenguaje favorito.';

echo 'Texto "' . $text . '" es una larga' . strlen($text) . 'personajes.';
```

PHP tiene muchas funciones predefinidas directamente (<a href="/documentación">vea la documentación para una lista completa</a>), pero a menudo es conveniente definir las propias:

```php
function mojeFunkce(int $x, int $y): int
{
   $z = $x + $y;   // añade los parámetros de entrada y los almacena en una variable

   return $z;      // devuelve la variable $z
}

echo mojeFunkce(5, 3); // imprime el número 8, porque los números fueron procesados por la función
```

Variables en las funciones
--------------------------

Las variables locales se utilizan dentro de las funciones, lo que significa que sólo pueden ser utilizadas dentro de la función y no pueden ser manipuladas (o definidas) fuera de la función. Obtienen sus valores iniciales de los parámetros de la función directamente en la definición de la misma.

Ejemplo:

```php
$z = 5;

function prvniFunkce(int $x, int $y): int
{
   return $x - $y; // esto devolvería la diferencia de números
}

function druhaFunkce(): mixed
{
   return $z; // esto devuelve un error porque la variable $z
              // no se define dentro de la función
}
```

A veces es útil establecer algunos de los parámetros como opcionales, esto se hace definiendo un valor alternativo (por defecto):

```php
echo prictiCislo(5);    // devuelve 6
echo prictiCislo(5, 7); // devuelve 12

function prictiCislo(int $x, int $y = 1): int
{
   return $x + $y;
}
```

La función `prictiCislo()` añade el valor de la variable `$y` a la variable `$x`. Si la variable `$y` no está definida (especificada como parámetro al llamar a la función), se utilizará su valor por defecto escrito en la definición de la función. El segundo parámetro de la función (la variable `$y`) es por tanto opcional.

> Cada función sólo puede tener una salida (un retorno), si se especifican varias salidas en una función, se devolverá la especificada en primer lugar. Si quiere devolver varios valores, debe utilizar un array.
>
> En PHP (a diferencia de otros lenguajes) no tenemos que especificar un retorno en absoluto, en cuyo caso la función no devuelve nada (void) en cualquier entrada. Estas funciones se utilizan sobre todo para almacenar datos o dar salida al código fuente. En general, sin embargo, se recomienda devolver siempre algún valor, al menos un reconocimiento de que todo ha ido bien.
