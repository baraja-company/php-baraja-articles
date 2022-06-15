Condiciones en PHP - IF() {...} - opciones de ramificación
==========================================================

> id: '3e4b81bb-a8bb-4e99-8842-e76f1658a371'
> slug:
> 	cs: if
> 	es: condiciones-en-php-if-...-opciones-de-ramificacion
> 
> perex:
> 	- 'Podmínky, logické výrazy, booleovská logika a možnosti větvení PHP scriptů. Vyhodnocování logických výrazů a operátory. Možnosti skládání výrazu.'
> 	- 'Condiciones, lógica booleana, lógica booleana y opciones de bifurcación del script PHP. Evaluación de expresiones y operadores booleanos. Opciones de plegado de la expresión.'
> 
> publicationDate: '2019-11-26 11:55:09'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '759a244b4fe2dc119c475aee42259578'

> Nota: Este artículo puede ser un poco confuso para algunos principiantes, ya que asume una comprensión básica de PHP. Si te interesa saber cómo funcionan las condiciones, lee <a href="/condiciones">sobre las condiciones en el curso para principiantes</a>.

| Soporte: Todas las versiones: PHP 4, PHP 5, PHP 7
|-------------------|----------|
| Breve descripción: | Validación de una o varias declaraciones
| Tipo: <a href="/declaraciones-y-funciones">Declaración, construcción</a> (no una función)

Descripción
-----

A menudo es necesario determinar si una igualdad se mantiene o si una afirmación es verdadera, para eso están las condiciones. PHP utiliza la siguiente sintaxis como muchos otros lenguajes (especialmente C):

```php
if (logický výrok) {
    // construir
}
```

Cada expresión lógica tiene un valor de `TRUE` (verdadero) o `FALSE` (falso), no hay otras opciones.

Ejemplo de comparación de si la variable `$x` es mayor que la variable `$y`:

```php
$x = 10;
$y = 5;

if ($x > $y) {
    // Esta parte del script se ejecuta si la condición es verdadera
} else {
    // Esta parte del script se ejecutará si la condición no se aplica
}
```

La construcción de la condición tiene un contenido obligatorio entre paréntesis, en el que se da la expresión que se va a probar, compuesta por operadores (resumen más abajo), se pueden enlazar múltiples expresiones utilizando operadores lógicos (resumen más abajo).

Además, la condición contiene dos bloques de declaraciones opcionales.

- Si se cumple la condición
- Si la condición no se cumple

Por razones prácticas, incluya siempre al menos el primer bloque de sentencias cuando la condición sea verdadera, ya que de lo contrario no tendría sentido probar la expresión.

Uso de puntos y comas y paréntesis
--------------------------

En general:
- Los paréntesis redondos** se utilizan para separar expresiones lógicas (se pueden poner otros paréntesis redondos para conseguir expresiones más complejas).
- El corchete **complejo** se utiliza para delimitar un bloque de comandos y funciones.
- El **medio** no se utiliza para indicar una condición (el bloque de comandos está delimitado por un corchete compuesto), sino para separar los comandos individuales dentro de la condición).

La única notación posible con un punto y coma (excepto cuando se utiliza la construcción **endif**):

```php
if ($x > $y);
```

Sin embargo, dicha condición no tiene sentido porque en ambos casos el resultado de la comparación será descartado y no se ejecutará ninguna sentencia perteneciente a la condición.

Notación alternativa
--------------------------

En determinadas circunstancias, la construcción "si" puede utilizarse omitiendo los corchetes compuestos. Esto sólo puede lograrse en los siguientes casos:

- Si ejecutamos sólo una declaración en la condición.
- Si utilizamos dos puntos y un endif en lugar de paréntesis compuestos;
- Si utilizamos la notación "en línea".

Para obtener información más detallada, consulte los 3 capítulos siguientes.

> **`1. Sólo un comando ~ sintaxis abreviada`**

Si está creando una condición en la que desea ejecutar sólo una construcción (declaración), puede utilizar la notación clásica de corchetes compuestos:

```php
if ($x > 10) { $y = $x; }
```

O podemos omitir los paréntesis:

```php
if ($x > 10) $y = $x;
```

Sin embargo, este comportamiento sólo se aplica a un comando en las inmediaciones de la condición.

Un ejemplo mejor (sólo la construcción `$y = $x` se ejecuta condicionalmente, el resto se ejecuta siempre):

```php
$x = 5;
$y = 3;
$z = 10;
if ($x > $y)
$y = $x;
$x = 3;
```

> **`2. Dos puntos y endif;`**

```php
if (výraz):
    konstrukt;
    konstrukt;
    konstrukt;
endif;
```

Sin embargo, hace tiempo que esta notación se considera obsoleta porque se reduce en orientación cuando se sumergen múltiples condiciones en sí mismas.

> **Nota:** Me gustaría señalar que este estilo también le gusta a algunas personas, como a Yuh (<a href="https://www.jakpsatweb.cz/php/php-tahak.html#vetveni">ver su artículo</a>). Dios no permita que uses esto en algún lugar.

> **`3. Expresión ternaria ~ notación "en línea" de una sola línea`**

Ocasionalmente es útil hacer una simple comparación en línea con alguna otra acción (por ejemplo, junto con la definición de una nueva variable). Si queremos ejecutar una sola sentencia, todo el procedimiento puede reducirse a una sola línea, aunque manteniéndolo lo más sencillo posible.

```php
$x = 5;
$xJeVetsiNezDva = ($x > 2 ? TRUE : FALSE);

// o incluso más corto:

$xJeVetsiNezDva = ($x > 2);
```

Tablas de operadores
-----------------

Dentro de la condición se utilizan dos tipos de operadores:

- **Comparativa** ~ comparan una relación específica,
- **Lógico** ~ combinar múltiples expresiones para crear condiciones complejas.

Operadores comparativos
-----------------------

| Operador: Significado.
|----------|----------|
| `==` | Igual a
| `===` | Es igual y tiene el mismo tipo de datos
| No es igual
| `>=` | Es igual o mayor
| `<=` | Es igual o menor que
| Mayor
| Menos

Ejemplo (válido cuando `$x no es 5`):

```php
if ($x != 5) { ... }
```

Operadores lógicos
--------------------

| Operador, Alternativa, Significado, Verdadero cuando..:
|-----------|--------------|----------------|--------------------------------------------
| `&&` | AND | y al mismo tiempo | ambos valores son verdaderos
| `||` | O | o | al menos un valor es verdadero
| `^^` | XOR | OR exclusivo | al menos uno es verdadero o falso, pero nunca ambos
| `!` | *no* | negación de la expresión | `true` cuando `false` y viceversa
| La negación de la expresión depende de las circunstancias.

Un ejemplo más complejo:

```php
$x = 5;
$y = 3;
$z = 8;
if ($x > 0 && !($y != 2 && $z == $x) || $z > $y) { ... }
```

Omisión de operadores lógicos y de comparación
---------------------------------------------

A menudo podemos permitirnos omitir cualquiera de los dos operadores (o incluso los dos), pero nunca debemos olvidar las reglas de uso correcto para que la expresión resultante funcione.

En general, cuando se comprueba una expresión sin operador, se comprueba si su valor es `TRUE` o no está vacío (por ejemplo, contiene un número distinto de cero, una cadena no vacía, ...).

Ejemplos:

```php
$x = 5;
$y = 3;
$z = 8;

if ($x) { ... }         // pasa porque $x no está vacío
if ($x && $y) { ... }   // pasa porque $x y $y no están vacíos
if (!$x) { ... }        // falla porque TRUE es negado
if (isset($z)) { ... }  // pasa porque la variable $z existe
```

Pero pueden surgir situaciones complicadas, especialmente cuando:
- Pido `si ($x)` y la variable `$x` contiene cero (`0`), entonces la condición no se cumple.
- O la variable `$x` contiene la cadena ``0`` (el número cero), porque se desborda a cero y la expresión, por tanto, no es verdadera.
- Hay una función en la condición que siempre devuelve alguna cadena no vacía, porque entonces la condición es siempre verdadera.
- O si estamos comprobando la entrada del usuario y éste devuelve `'false'` como cadena, entonces de nuevo la condición es verdadera porque la cadena no está vacía.

Recomiendo una solución sencilla y eficaz para esto: pedir el número de caracteres que se devuelven. Si la cadena está vacía (o la variable no existe), se devuelven cero caracteres y no se cumple la condición. Un ejemplo sencillo:

```php
$x = '0';
if ($x) { ... }			// la condición no se aplica normalmente
if (strlen($x)) { ... }	// la condición es válida porque $x contiene 1 carácter
```

A continuación, podemos comprobar la existencia de una variable mediante la función `isset()`.

Comparación de cadenas
-----------------

Descubrir que las cuerdas son idénticas es fácil:

```php
$a = 'Gato';
$b = 'cat';

if ($a === $b) {
    // Si las cadenas son iguales
} else {
    // Si las cadenas son diferentes
}
```

Es importante vigilar bien los tipos de datos por si la entrada pudiera ser equivalente a alguna otra.

Por ejemplo, la cadena vacía `$a = '';` es diferente de la cadena `NULL`: `$b = NULL;`. Necesitamos hacer esta distinción, por ejemplo, debido a las bases de datos en las que hay una diferencia entre un valor que no existe o que está vacío.

```php
$a = '';
$b = null;

if ($a == $b) {
    // Se evaluará como TRUE porque
    // se convierte el tipo de datos.
}

if ($a === $b) {
    // Realiza una validación mucho más rigurosa
    // y no pasará porque es un
    // contenido y un tipo de datos diferente, por lo tanto
    // este código nunca se ejecutará.
}
```

También es una buena idea ignorar los caracteres blancos (invisibles) como los espacios, los tabuladores y los saltos de línea cuando se comparan las cadenas. Esto es útil, por ejemplo, al introducir una contraseña y pasarla a una función de hash:

```php
$password = '81dc9bdb52d04dc20036dbd8313ed055'; // 1234
$userPassword = '1234';

if (md5(trim($userPassword)) === $password) {
    // La función trim() elimina automáticamente los espacios.
}
```

¿Valor desconocido (inexistente)?
--------------------------

A veces puede ocurrir que el valor no exista (no es ni `TRUE` ni `FALSE`), se trata principalmente de un valor obtenido de la base de datos (por ejemplo, estamos pidiendo una columna que no existe), en este caso se devolverá el tipo de dato `NULL`.

En general, `NULL` se evalúa como `FALSE`, es decir, la condición no se aplica. Sin embargo, este comportamiento no siempre es conveniente, ya que un valor inexistente no significa necesariamente que no haya ningún registro.

> Ejemplo de la práctica: Tenemos un perfil de usuario y consultamos la página web del usuario. No todos los usuarios necesitan tener una página web, así que en este caso se devuelve `NULL`, pero el usuario sigue existiendo. Así que en este caso, deberíamos utilizar la función `isset()` para comprobar la (no) existencia de la variable y no sacar una conclusión basada en un valor específico.
