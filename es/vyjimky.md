Excepciones y su captura en PHP
===============================

> id: '61b0f178-bb1c-4166-9e8a-af49de2e2a8c'
> slug:
> 	cs: vyjimky
> 	es: excepciones-y-su-captura-en-php
> 
> publicationDate: '2020-02-16 22:18:18'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: d843fe11a092943db429cb8a28384a31

Las excepciones son las herramientas de la programación orientada a objetos, que proporciona una forma elegante de lanzar y manejar (tratar) los errores de la aplicación.

Una excepción se lanza primero (`thrown`), se trata (`try`) y se atrapa (`catch`). Sólo el lanzamiento es obligatorio.

Filosofía de generación de excepciones
-------------------------

Antes de la aparición de las excepciones, la gestión de errores en la programación era muy complicada porque teníamos que confiar en los valores de retorno de las funciones y atraparlos a nuestra manera y comportarnos en consecuencia.

De hecho, las funciones en sí mismas no imponen el manejo de errores, lo que puede llevar a problemas fatales, pero David ha escrito sobre esto en <a href="https://phpfashion.com/programatori-chyby-neignoruji">Los programadores no ignoran los errores</a>.

Un ejemplo de gestión de errores olvidados:

```php
// pasar de un disco a otro
copy('c:/archivo antiguo', 'd:/archivo nuevo');
unlink('c:/archivo antiguo');
// si la primera operación falla, el archivo se borra irremediablemente
```

Esto se debe a que la forma correcta de manejar la salida de la función `copy()` es no continuar y lanzar un error. En el caso de las viejas funciones, podría ser así:

```php
function backup(): bool
{
   if (copy('c:/archivo antiguo', 'd:/archivo nuevo')) {
      return unlink('c:/archivo antiguo');
   }

   return false;
}
```

Nuestra función `backup()` devolverá `true` sólo si la función `copy()` no falló y la función `unlink()` devolvió `true`. En caso contrario, devolverá `false`.

Pero, ¿es esto ahora seguro para la aplicación? No lo es. Porque ahora tenemos que tratar la salida de la función `backup()` en el momento en que la llamamos, y si falla, ni siquiera sabremos por qué. En resumen, devolverá `false` y tendremos que detectar el error nosotros mismos de alguna manera. Supongo que en este caso es bueno ver que los programadores a menudo abandonan el manejo de errores, o simplemente se olvidan de manejar algo y la aplicación lanza errores difíciles de detectar por ello.

La solución a este problema es utilizar excepciones que fuerzan el tratamiento, y si no se tratan, la aplicación se bloquea por completo y siempre descubrimos el motivo.

Definición básica de la excepción
--------------------------

En PHP, una excepción es una clase especial de interfaz implementada por la clase nativa `Exception` que vamos a utilizar.

Si el procesamiento de alguna parte del programa falla, simplemente lanzamos una excepción describiendo el problema:

```php
if (copy('c:/archivo antiguo', 'd:/archivo nuevo') === false) {
   throw new \Exception('No se puede copiar el archivo "oldfile".');
}
```

Lanzar una excepción se hace con la palabra clave `throw`, seguida de la creación de una instancia de la clase con la excepción. También podemos obtener una instancia de otras maneras (por ejemplo, pasándola desde una variable), y el hecho de crear una instancia de excepción no hace que se lance.

El primer argumento del constructor de la clase ``Exception`` acepta el texto de la excepción, que debe explicar de forma concisa lo que acaba de suceder. Es una buena práctica incluir también información sobre la operación que se realiza y una referencia a los datos. Por ejemplo, si una copia de archivo ha fallado, es una buena práctica pasar el nombre del archivo. Si la ejecución de la consulta SQL falla, volvemos a pasar la consulta que se está ejecutando. Esto nos ayudará mucho más adelante a la hora de gestionar los errores, porque podemos ver exactamente cuál es el problema.

Gestión de excepciones
-----------------

Por ejemplo, tengamos una función `backup()` que hace una copia de seguridad de los datos y puede lanzar un par de errores:

```php
function backup(): void
{
   if (copy('c:/archivo antiguo', 'd:/archivo nuevo')) {
      if (unlink('c:/archivo antiguo') === false) {
         throw new \Exception('No se puede eliminar el archivo antiguo.');
      }
   }

   throw new \Exception('No se pueden copiar los archivos de copia de seguridad.');
}
```

Nótese que la función no devuelve ninguna salida, y que especificamos el tipo `void` en la definición. La función no necesita devolver nada, porque el éxito se considera un estado en el que no se lanza ningún error y no necesitamos tratar un escenario positivo.

Si utilizáramos la función en una aplicación sin tratamiento, por ejemplo, de la siguiente manera:

```php
echo 'Copia de seguridad de los archivos...';
backup();
echo 'Copia de seguridad completa.';
```

Esa es la forma normal en que funcionará. Sin embargo, si se produce un error, la secuencia de comandos terminará automáticamente y el texto de la excepción se mostrará en la salida. Lo más importante es que no continuará ejecutando el código y sabemos que no se producirá ninguna corrupción de datos.

Si queremos continuar la ejecución, necesitamos **limpiar** el error, lo que hacemos utilizando las construcciones `try` y `catch`:

```php
echo 'Copia de seguridad de los archivos...';
try {
   backup();
} catch (\Exception $e) {
   echo 'La copia de seguridad ha fallado:' . $e->getMessage();
}
echo 'Copia de seguridad completa.';
```

Si se lanza una excepción, se llama al código del área `catch()` (que acepta la excepción si coincide con su tipo de datos) y se ejecuta el código interno.

Siempre obtenemos una instancia de la clase de excepción, que puede ser utilizada, por ejemplo, para mostrar un mensaje de error, que es manejado por el método `getMessage()`. También es útil conocer el método `getFile()`, que devuelve la ruta del disco al archivo que contiene el error, `getCode()`, que devuelve el código de estado del error, y `getLine()`, que devuelve el número de línea donde se lanzó la excepción.

Excepciones preparadas
------------------------

Además de la excepción básica `\NException`, PHP incluye otros tipos de excepción predefinidos que son adecuados para diferentes casos de uso.

| Tipo de datos: Explicación
|------------|-----------|
| `LogicException` | Error lógico, predecible en el diseño del programa |
| `BadFunctionCallException` | Error de llamada de función; función no encontrada; llamada no permitida |
| `BadMethodCallException` | Error de llamada de método |
| `InvalidArgumentException` | Argumento incorrecto (no válido) pasado a la función o método |
| `OutOfRangeException` | Valor fuera del rango de la matriz o colección |
| `LengthException` | El valor excede la longitud permitida |
| `DomainException` | El valor no entra en el dominio o rango requerido |
| `RuntimeException` | Error sólo detectable en tiempo de ejecución (por ejemplo, fallo al escribir en un archivo) |
| `OverflowException` | Desbordamiento del búfer o de la operación aritmética, a menudo causado por el procesamiento de más datos de los esperados |
| `UnderflowException` | Desbordamiento de un búfer o de una operación aritmética, se han pasado menos datos de los esperados |
| `OutOfBoundsException` | Índice fuera del rango de la matriz o colección |
| `RangeException` | El valor no está dentro del rango solicitado |
| `UnexpectedValueException` | Valor inesperado (por ejemplo, el valor de retorno de una función) |

Las excepciones `LogicException` y `RuntimeException` deberían evitarse mediante un diseño adecuado del programa. Personalmente, los utilizo sólo para situaciones excepcionales, como el fallo de escritura en un archivo y la comunicación con un servicio externo.

Recomiendo no capturar `RuntimeException` en absoluto y dejar que la aplicación falle. Suele tratarse de un problema grave que debe denunciarse lo antes posible.
