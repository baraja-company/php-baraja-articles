Ciclos y sus tipos en PHP
=========================

> id: e2927790-d0fb-4de5-9848-01fdd088464b
> slug:
> 	cs: cykly
> 	es: ciclos-y-sus-tipos-en-php
> 
> perex:
> 	- 'Cykl for, while, foreach a rekurze v PHP + vysvětlení rozdílů mezi cykly. Možnosti iterativního zpracování úloh.'
> 	- 'Ciclo for, while, foreach y recursividad en PHP + explicación de las diferencias entre ciclos. Posibilidades de tratamiento iterativo de las tareas.'
> 
> publicationDate: '2020-04-11 18:56:34'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '297931d6e7b8c9e011dbbe06b414cdde'

Los bucles en general se utilizan para repetir la misma sección de código (normalmente sobre un conjunto de datos) Para cada tipo de tarea, es adecuado un tipo diferente de bucle, que difiere principalmente en el significado y la sintaxis. También es importante señalar que todos los tipos de bucles son convertibles entre sí, sólo que no siempre merece la pena en términos de complejidad del código y tiempo de cálculo.

La división básica de los bucles en términos de tipo de elemento:

- `for`: Un bucle genérico en el que conocemos el número de repeticiones por adelantado, o simplemente podemos calcularlo por adelantado,
- `mientras` y `hasta-mientras`: Un bucle general en el que no conocemos de antemano el número de repeticiones,
- `foreach`: Navegar por arrays y objetos,
- `recursión`: Descomponer un problema complejo en un conjunto de problemas más pequeños.

Propiedades generales de los bucles
-----------------------

Los ciclos generalmente funcionan repitiendo un trozo de código marcado **una y otra vez**, **mientras la condición de terminación** se mantenga. El bucle se detiene si no se cumple la condición de finalización, o si se detiene manualmente llamando a `break;`.

Si nada detiene el bucle, entonces nada impide que se ejecute indefinidamente, o hasta que la aplicación se termine manualmente, o se agote el límite de tiempo para procesar el script PHP (normalmente 30 segundos).

**Términos importantes:**

- `Cycle` - una expresión del lenguaje que permite repetir una parte determinada del código
- `Iteration` - una ejecución específica del cuerpo del bucle
- `Inicialización` - iniciar la ejecución de un bucle (antes de que comience la primera iteración)
- `Increment` - Incrementar en uno una variable de iteración
- Condición de salida - Una condición que se verifica en cada iteración (la ubicación de la verificación varía según el tipo de bucle). El bucle se ejecuta mientras la condición se mantenga

Bucle For
----------

`For` es útil para los casos en los que conocemos el número de repeticiones de antemano (o podemos calcularlo fácilmente). Es perfecto para recorrer linealmente los intervalos, por ejemplo.

Generalmente se escribe (3 partes):

```php
for (inicializace; výraz; inkrementace) {
    // el cuerpo del ciclo
}
```

- `Inicialización`: Definición del estado inicial del bucle (qué variables necesitamos),
- `expresión`: Condición. Mientras sea válido, el bucle se ejecutará,
- `increment`: Cambia el estado del bucle desde la pasada anterior (`increment` - incrementa en uno).

Un ejemplo sería dar salida a una serie de números del `0` al `10`:

```php
for ($i = 0; $i <= 10; $i++) {
    echo $i . '<br>';
}
```

O múltiplos de dos desde `2` hasta `100`:

```php
for ($i = 2; $i <= 100; $i += 2) {
    echo $i . '<br>';
}
```

El bucle for es útil para varios trucos, como <a href="/abeceda-cisla-intervalos">obtener el alfabeto, un array de números e intervalos</a>.

Mientras un bucle do-while
-----------------------

Un bucle `While` se ejecuta mientras la condición se mantenga. La diferencia entre `while` y `do-while` es cuándo se evaluará la condición.

```php
while (výraz) {
    // el cuerpo del ciclo
}
```

Alternativamente:

```php
do {
    // el cuerpo del ciclo
} while(výraz);
```

- El `mientras` primero evalúa la condición y luego ejecuta el bucle interno,
- `do-while` primero procesa el bucle interno y luego evalúa la condición.

Esto es especialmente ventajoso cuando no podemos saber de antemano si el bucle se ejecutará alguna vez y queremos garantizar que siempre se ejecute al menos una vez (entonces usamos `do-while`).

Ejemplo:

> Tenemos un número almacenado en la variable `$x = 2`. También queremos generar un número en el intervalo `<0; 10>` en la variable `$y` para que sea diferente al número en la variable `$x`. En pocas palabras: Genera un número aleatorio entre `0` y `10` que no sea `2`.

En este caso, es conveniente utilizar un bucle `do-while`. No sabemos de antemano cuántas veces tendrá que ejecutarse el bucle (podemos tener mala suerte y seguir dando con el mismo número, en cuyo caso el bucle se ejecutará durante mucho tiempo), y además queremos garantizar que se ejecutará al menos una vez antes de que se evalúe la condición, ya que primero tenemos que generar el número.

```php
$x = 2;
$y = $x;

do {
   $y = rand(0, 10);
} while($x == $y);

echo 'X :' . $x . '<br>';
echo 'Y:' . $y;
```

Para cada
-------

`foreach` es perfecto para explorar campos y objetos. Podemos obtener no sólo valores específicos, sino también claves.

Si sólo queremos obtener valores específicos:

```php
foreach ($data as $value) {
    // tratamiento de datos
}
```

O bien, podemos conseguir las llaves:

```php
foreach ($data as $key => $value) {
    // tratamiento de datos
}
```

`foreach` es perfecto para navegar por datos reales - por ejemplo, resultados de una base de datos, o campos con claves:

```php
$countries = [
    'en' => 'República Checa',
    'Ver' => 'Eslovaquia',
];

foreach ($countries as $shortcut => $name) {
    echo $shortcut . ':' . $name . '<br>';
}
```

Recursión
-------

La recursión se produce cuando una función o método se llama a sí mismo. Sirve para descomponer un problema complejo en un grupo de problemas más pequeños.

> Entender la recursividad puede ser un reto para los principiantes porque se basa en la idea de pasar responsabilidades.
>
> La función en realidad dice algo así como "No puedo resolver este problema, pero conozco a alguien que puede...", así que lo llama, él llama a alguien, ... hasta que se llama al miembro final, y corta el problema.

Sin duda, vale la pena señalar que cualquier algoritmo recursivo puede ser reescrito para utilizar bucles donde la recursión no es necesaria (lo contrario también es cierto). La recursión es un buen sirviente, pero un mal maestro. Ayuda a resolver algunos tipos de problemas de forma sencilla y muy eficiente, mientras que el bucle de ciclos es útil para otras cosas.

En general, la recursividad hace un uso intensivo de la memoria porque crea nuevas instancias y contextos de funciones y métodos todo el tiempo. Sin embargo, para recorrer estructuras arbóreas, por ejemplo, ésta es la única forma razonable de resolver el problema.

Ejemplo:

Necesitamos calcular el factorial del número `5`, así es como se hace en matemáticas:

¡`5! = 5 * 4 * 3 * 2 * 1`

Es decir, hay una multiplicación sucesiva de una serie de números desde `1` hasta el número del que nos interesa el factorial.

De forma recursiva, esto se puede resolver de forma muy elegante:

```php
function factorial(int $n): int
{
   if ($n === 0) {
      return 1;
   }

   return $n * factorial($n - 1);
}
```

También se puede realizar una implementación aún más corta utilizando el operador ternario:

```php
function factorial(int $n): int
{
    return $n === 0 ? 1 : $n * factorial($n - 1);
}
```

Lo mismo se puede reescribir para utilizar ciclos:

```php
function factorial(int $n): int
{
    $result = 1;

    for ($i = $n; $i > 0; $i--) {
        $result *= $i;
    }

    return $result;
 }
```

La escritura con un ciclo es ligeramente más larga, pero de nuevo tiene una huella de memoria significativamente menor. Utilizamos una sola variable `$resultado` para el cálculo, donde cambiamos el valor continuamente y no tenemos que crear nuevas instancias de `factorial()`.

Escriba los bucles infinitos con mucho cuidado
-----------------------------------

Puede ser muy fácil que un bucle se convierta en infinito (lo conseguimos al no satisfacer nunca la condición de terminación). Debería tener esto en cuenta y posiblemente detener el bucle usted mismo con `break;`.

Por ejemplo, tira el dado hasta que salga el número `6`. La implementación se ve tentada a utilizar el bucle `mientras`:

```php
while (true) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'El valor ha caído' . $value;
      break;
   }
}
```

Al escribir `mientras(verdadero)` nos hemos asegurado un número infinito de repeticiones, porque `verdadero` siempre será verdadero. Así que tenemos que detener el bucle nosotros mismos con la construcción `break;`, pero ¿qué pasa si el valor `6` nunca cae y nunca detenemos el bucle?

Personalmente, siempre cuento el número de repeticiones y si supera algún límite crítico, termino el bucle a la fuerza. Por ejemplo, si superamos los `1000` intentos. En ese caso, es mejor utilizar `for` en lugar de `while` y contar el número de ejecuciones.

Si superamos el número de ejecuciones, es educado informar al desarrollador lanzando una excepción.

```php
for ($i = 0; $i <= 1000; $i++) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'El valor ha caído' . $value;
      break;
   }
}

if ($i === 1000) {
   throw new \Exception('Se ha superado el número máximo de lanzamientos.');
}
```
