Matemáticas en PHP
==================

> id: '69a432ee-7635-46e2-bb21-8492cb62c4e6'
> slug:
> 	cs: matematika
> 	es: matematicas-en-php
> 
> perex:
> 	- 'Matematika a matematické funkce v PHP. Definice čísel, operátorů a vhodné typy pro výpočty a ukládání čísel.'
> 	- 'Matemáticas y funciones matemáticas en PHP. Definiciones de números, operadores y tipos adecuados para los cálculos y el almacenamiento de números.'
> 
> publicationDate: '2020-02-16 18:24:10'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '1070f11c80eff1faf3388e47721b01a2'

Al igual que en otros lenguajes, los números en PHP se representan en el sistema binario (el sistema de unos y ceros), por lo que algunas operaciones matemáticas pueden producir resultados ligeramente diferentes a los que esperaríamos. Esta página ofrece una visión general del comportamiento de los cálculos en diferentes contextos. Se requiere al menos un conocimiento básico de **técnicas numéricas** y **sistemas numéricos** para su comprensión.

Representación de números en la memoria
---------------------------

La forma de representar los números se caracteriza por el tipo de datos, por ejemplo los enteros se convierten directamente desde el código fuente de decimal a binario. No tenemos que preocuparnos en absoluto de convertir los números, todo se hace automáticamente.

La consecuencia de esta conversión es que el valor máximo de un entero viene determinado por el número de bits disponibles en la memoria. En el PHP de 32 bits el rango va de `-2.147.483.648` a `-2.147.483.647` (~ ± 2 mil millones), el PHP de 64 bits tiene un rango de `-9.223.372.036.854.775.808` a `-9.223.372.036.854.775.807` (~ ± 9 quintillones). El valor máximo se puede obtener siempre llamando a la constante `PHP_INT_MAX`.

Los números decimales se almacenan en el tipo de datos **float**, que tiene una precisión limitada, por lo que se almacena internamente como un par de números que están sujetos a la operación matemática `mantisa * (2^exponente)`. La consecuencia práctica es que podemos almacenar un número relativamente grande (del orden de miles de millones) en un número reducido de bits, sólo que no con mucha precisión.

La consecuencia de utilizar el tipo de datos **float** es que el resultado de calcular `1 - 0,9` no es exactamente `0,1`.

Ejemplo de <a href="https://diskuse.jakpsatweb.cz/?action=vthread&forum=9&topic=97912">foros web</a>:

```php
$x = 0.3;
$y = 0.4;
echo 0.7 - $x - $y; // imprime -5.5511151231258E-17
```

Si ajustamos los números ligeramente:

```php
$x = 6.5;
$y = 7.5;
echo 14.0 - $x - $y; // imprime 0
```

En general, esta cuestión se <a href="https://vtm.zive.cz/clanky/proc-pocitacum-delaji-problemy-desetinna-cisla/sc-870-a-185672/default.aspx">discute en un artículo aparte en VTM.e15.cz: Por qué los ordenadores tienen problemas con los números decimales</a>, o en general sobre <a href="https://cs.wikipedia.org/wiki/Pohyblivá_řádová_čárka">punto flotante en Wikipedia</a>.

> Generalmente, es una buena idea redondear el resultado después del cálculo, o evitar los decimales por completo. Por ejemplo, guarde el precio no en coronas (como 14,90) sino en centavos (como 1490) y luego trabaje con él exactamente. El redondeo se trata en la siguiente sección de este artículo.

Operaciones matemáticas
-------------------

```php
$a = 5;
$b = 3;

echo $a + $b;
```

Para las operaciones se pueden utilizar convenciones matemáticas comunes, que respetan la precedencia de los operadores según las reglas de las matemáticas (la multiplicación tiene prioridad sobre la suma, etc.). Los valores pueden escribirse directamente o leerse a través de variables.

> Puede que no sea obvio a primera vista, pero en el ejemplo de matemáticas, el signo de igualdad (`=`) no está escrito. Por lo tanto, se deduce que sólo se pueden resolver `operaciones binarias` simples, que siempre funcionan con sólo `dos elementos` (no cuentes ecuaciones, tienes que usar una biblioteca especializada para eso).

Resumen de los operadores básicos
----------------------------

> En la columna de `resultados`, enumero lo que se imprime suponiendo:

```php
$a = 5;
$b = 3;
$c = 10;
```

| Operación | Marca | Marca en palabras | Ejemplo de notación | Resultado
|-------------------|-----------|---------------|-----------------------|----------
| Adición | `+` | Más | `echo $a + $b;` | 8
| Sustracción | `-` | Menos | `echo $a - $b;` | 2
| Multiplicación | `*` | Asterisco | `echo $a * $b;` | 15
| división | `/` | barra inclinada | `echo $a / $b;` | 1.66666666667
| paréntesis | `( )` | | `echo $a + ($b * $c);`| 35
| Concatenación de cadenas | `.` | Punto | `echo $a . $b . $c;` | 5310
| Resto después de la división | `%` | Porcentaje | `echo $a % $b;` | 2
| Añadiendo uno | `++` | dos pluses | `echo $a++;` | 6
| Resta uno | `--` | dos menos | `echo $a--;` | 4
| Poder | `**` | dos asteriscos | `echo $a ** $b;` | 125

> El operador de potencia (doble asterisco) sólo está disponible a partir de PHP 7.1. En otras versiones de PHP hay que utilizar la función universal `pow($a, $b)`.

Resumen de las funciones matemáticas básicas
---------------------------------------

Si está resolviendo alguna tarea computacional común, lo más probable es que ya exista una función directamente en PHP, aquí hay un resumen anotado de ellas.

Redondeo
--------------

El redondeo normal se realiza con la función <a href="/función-redondear">redondear()</a>, tiene 3 parámetros:

- Número redondeado
- Opcional: Precisión (con cuántos decimales)
- Opcional: Reducir a la mitad el valor (a partir de qué valor redondear)

```php
round(5);               // 5
round(5.1);             // 5
round(5.4);             // 5
round(5.5);             // 6
round(5.8);             // 6
round(5483.47621, 2);   // 5483.47
round(5483.47621, -2);  // 5500
```

También hay una función <a href="function-floor">floor()</a> para redondear hacia abajo y una función <a href="/function-ceil">ceil()</a> para redondear hacia arriba.

> **¡Cuidado con los tipos de datos!**
>
> Todas las funciones de redondeo devuelven el resultado como `float`, incluso si el resultado es un entero.
>
> > Si quieres tratar el resultado como un entero, es importante reescribir el resultado después. Por ejemplo: `echo (int) round(3.14);`

PHP trabaja con diferentes tipos de datos, por ejemplo con **float** puede ocurrir fácilmente que el resultado de `round()` no sea un número con expansión decimal finita. Para listar, recomiendo usar la función `number_format()`, que discuto más abajo.

Funciones goniométricas
--------------------

Se utilizan para muchos cálculos diferentes y se basan en el círculo unitario. Se utilizan, por ejemplo, para trazar círculos, elipses, desplazamientos, etc.

- <a href="/función-sin">sin()</a>
- <a href="/función-cos">cos()</a>
- <a href="/función-tan">tan()</a>

Funciones ciclométricas
--------------------

Calcula el ángulo de `x` e `y`. Conversiones de coordenadas cartesianas y polares.

- <a href="/función-asin">asin()</a>
- <a href="/función-acos">acos()</a>
- <a href="/función-atan">atan()</a>

Funciones hiperbólicas
-------------------

Basado en la hipérbola unitaria. Sus definiciones pueden describirse fácilmente utilizando símiles:

- Elipse, Círculo, Círculo con radio 1
- Hipérbola, Hipérbola equiaxial, Hipérbola unitaria

Se utilizan, por ejemplo, en la generación de terrenos y en la simulación física.

- <a href="/función-sinh">sinh()</a>
- <a href="/función-cosh">cosh()</a> (chainline)
- <a href="/función-tanh">tanh()</a>

Funciones hiperbolométricas
------------------------

Basado en la hipérbola unitaria.

- <a href="/función-asinh">asinh()</a>
- <a href="/función-acosh">acosh()</a>
- <a href="/función-atanh">atanh()</a>

Ejemplos de uso de funciones goniométricas
---------------------------------------

```php
echo sin(30);            // -0.98803162409286
echo sin(deg2rad(30));   // 0.5
echo cos(deg2rad(123));  // -0.54463903501503
echo 1/tan(deg2rad(45)); // 1 (onen cotangente)
echo sin(deg2rad(500));  // 0.64278760968654
echo atan2(deg2rad(50), deg2rad(23)); // 1.1396575860761
```

Calculadora - procesamiento de expresiones matemáticas
--------------------------------------------

A veces puede ocurrir que necesitemos procesar una expresión matemática como una cadena de usuario, por ejemplo `5+2^(1+3/2)`.

No hay una manera fácil de procesar dicha entrada en PHP, pero he programado una serie de bibliotecas que abordan este tema.

Para más detalles, vea el artículo separado <a href="/pokrocila-calculator">Calculadora en PHP: Procesando una expresión matemática como una cadena</a>.

Operaciones con números grandes y precisión de los cálculos
------------------------------------------

Los números grandes y los decimales se almacenan como flotantes en PHP, y además se redondean a unos 15 decimales, lo que a veces puede ser molesto. Por eso se creó la biblioteca **<a href="https://www.php.net/manual/en/book.bc.php">BCMath Arbitrary Precision Mathematics</a>**.

Los números se representan como **cadenas** en esta biblioteca, por lo que no están limitados por la imprecisión de **float**, pero las operaciones que se realizan son varios órdenes de magnitud más lentas (pero siguen siendo más rápidas que si programamos nosotros mismos dicha función).

Por ejemplo, si queremos obtener una raíz cuadrada de 2 a 3 decimales, sólo tenemos que llamar a

```php
echo bcsqrt('2', 3); // 1.414
```
