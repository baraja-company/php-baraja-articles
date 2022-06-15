Operadores ternarios en PHP (?:) - condición en una línea
=========================================================

> id: '1191be9b-ff9d-4725-b0b4-17b60de2b935'
> slug:
> 	cs: ternarni-operator
> 	es: operadores-ternarios-en-php-condicion-en-una-linea
> 
> perex:
> 	- Ternární operátor umožňuje zapsat jednoduchou podmínku do jednoho řádku jako výraz.
> 	- El operador ternario permite escribir una condición simple en una línea como expresión.
> 
> publicationDate: '2019-11-26 11:59:18'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: '491dbbbaeceba356a030e2501f5c0e2b'

El operador ternario permite acortar una condición simple a una sola línea en un lugar donde el análisis sintáctico es innecesario, complejo o directamente inapropiado.

TL;DR
------

Los operadores ternarios son una abreviatura de las condiciones "si" y "si", por lo que no es necesario utilizarlos. Son útiles para piezas de lógica de verificación que se repiten constantemente. Utilice siempre el formato `(condición ? lógica positiva : lógica negativa)` incluyendo paréntesis. Se utiliza para la verificación breve para que el código sea más claro y corto.

Definiciones básicas
------------------

A menudo tenemos una condición de la forma, por ejemplo:

```php
$a = 5;
$b = 3;

if ($a > $b) {
     echo 'Es más grande';
} else {
     echo 'Es más pequeño';
}
```

Por lo tanto, si queremos escribir una simple frase, tenemos que utilizar 4 líneas de código, que podrían reducirse a una.

```php
$a = 5;
$b = 3;

echo 'Es' . ($a > $b ? 'más grande' : 'más pequeño');
```

Generalmente, el operador ternario se escribe utilizando 3 partes (por eso se llama "ternario"):

```php
(podmínka ? pokud je pravda : pokud není pravda)
```

Los operadores ternarios se utilizan muy a menudo en la práctica, por ejemplo para denotar filas pares en una tabla:

```php
$pole = [3, 1, 4, 1, 5, 9, 2];

for ($i = 0; $pole[$i]; $i++) {
     echo '<tr class=""' . ($i % 2 ? 'de' : 'incluso') . '">';

          // Esto es de alguna manera trabajar con una hoja de cálculo...
          // por ejemplo: echo '<td>' . $campo[$i] . '</td>';

     echo '</tr>';
}
```

Ejemplo de utilización del operador ternario
------------------------------------

A menudo necesitamos comprobar la existencia de un valor variable y utilizarlo inmediatamente si es necesario. Si no existe, queremos devolver el valor por defecto.

El enfoque clásico es hacerlo así:

```php
$a = 5;
$b = 8;

$c = $a ? $a : $b;
```

Si la variable `$a` existe, utiliza `$a` como valor, en caso contrario `$b`.

Sin embargo, a veces obtenemos el valor de una función:

```php
$a = 5;
$b = 3;
$default = 42;

$c = funkce($a, $b) ? funkce($a, $b) : $default;
```

Este método de llamada es muy ineficiente en términos de recursos del sistema. Primero hay que llamar a la función, y si existe, se llama de nuevo para obtener el valor, que se almacena en la variable `$c`.

Esto podría manejarse mejor a través de una variable de ayuda:

```php
$a = 5;
$b = 3;
$pomocnaPromenna = funkce($a, $b);
$default = 42;

$c = $pomocnaPromenna ? $pomocnaPromenna : $default;
```

Uso inadecuado
------------------

No siempre vale la pena utilizar el operador ternario porque tiende a causar confusión al escribir condiciones complicadas y anidadas.

Vea usted mismo un ejemplo:

```php
$valid = true;
$lang = 'Francés';

$x = $valid
     ? ($lang === 'Francés' ? 'oui' : 'sí')
     : ($lang === 'Francés' ? 'no' : 'de');
```

¿Sabrías a simple vista que la variable `$x` contendría el valor `oui`?

Después de un poco de práctica puede que sí, pero esa no es la respuesta correcta. :)

Verificación de la (no) existencia del valor y uso del valor por defecto
--------------------

Los operadores ternarios son más potentes en la verificación rutinaria de la (no) existencia de valores y el uso de otros valores por defecto.

Por ejemplo, queremos comprobar si existe una categoría principal para un artículo y, en caso contrario, emitir un mensaje de sustitución:

```php
echo $mainCategory ?? 'La categoría no existe';
```

El operador `??` (dos signos de interrogación) comprueba si la variable `$mainCategory` existe y no es `null`. Funciona de la misma manera que la función `isset()`.

Validar el vacío de un valor
-----------------------------

Una construcción muy útil para verificar que el valor de salida no está vacío (es decir, no es `null`, `0`, `false` o `''*(cadena vacía)*).

Esto se puede escribir de la siguiente manera:

```php
$a = 5;
$b = 3;
$default = 42;

$c = funkce($a, $b) ?: $default;
```

Primero se llama a la `función($a, $b)`, luego se comprueba su valor y si no está vacío, se pasa inmediatamente a la variable `$c`, en caso contrario se utiliza la variable `$default`.

El operador `?:` funciona como el operador `!=` en una condicional (falso independientemente del tipo de datos), y puede recordarse por su aspecto de cara sonriente con pelo de Elvis, por ejemplo.

Soporte para versiones antiguas de PHP
----------------------------

El operador `?:` funciona desde PHP 7. En versiones anteriores, tenemos que conformarnos con la condición `if (...)`, que puede lograr el mismo comportamiento. Recuerda que los operadores ternarios son en realidad un atajo para escribir lo mismo que se maneja con las condiciones.

La no existencia de un valor se puede comprobar con la función `isset()`, que comprueba que la variable existe y no está vacía (`null`).

En lugar de código:

```php
$a = 5;
$b = 3;

$c = $a ?? $b;
```

Luego escribimos la alternativa más antigua:

```php
$a = 5;
$b = 3;

$c = isset($a) && $a ?? $b;
```

> **Atención:** El orden de `isset()` y la propia variable importan. Si invirtiéramos el orden y la variable no existiera, se produciría un error de acceso a la variable inexistente.
