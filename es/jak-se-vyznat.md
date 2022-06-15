Cómo entender el código PHP
===========================

> id: d8d452da-37b4-4502-a573-a27afe7ea37a
> slug:
> 	cs: jak-se-vyznat
> 	es: como-entender-el-codigo-php
> 
> publicationDate: '2020-04-11 18:45:23'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c3012a19e6dd01770fad9ab456a5b6b3

La mayoría de las lenguas pueden escribirse de diferentes maneras, con el mismo resultado. Al mismo tiempo, una vez que haya escrito el código, probablemente lo leerá en algún momento en el futuro y lo corregirá o añadirá nuevas características.

Por lo tanto, para evitar tener que pensar en el código todo el tiempo y navegarlo bien, hay un conjunto de herramientas y formas de "escribir código bien" directamente en PHP, o de construir código de una manera que apoye directamente su futura legibilidad (incluso por otro humano).

> **Nota del autor:**
>
> La experiencia demuestra que el código se vuelve obsoleto tan rápido que incluso el propio autor de la aplicación percibe su propio código como extraño después de medio año. Así que si lo escribimos correctamente desde el principio, no impedirá su futura extensibilidad.
>
> En el desarrollo real, se demuestra en secreto que la uniformidad del formato del código y la introducción de reglas en general evitan una serie de errores.

TL;DR
-----

La legibilidad del código suele estar relacionada con el formato y las reglas de escritura.

Dentro de los equipos de desarrollo, tiene sentido establecer reglas formales sobre cómo formatear y mantener el código.

Personalmente, utilizo (en 2022) el estándar de codificación del framework Nette y las reglas se evalúan automáticamente en cada commit. Consulte el artículo sobre [el uso de GitHub CI](https://php.baraja.cz/github-actions-nejlepsi-ci-pro-rok-2021#hotove-priklady) para obtener más información.

La instalación de la prueba del estándar de codificación y su ejecución se realiza con un par de comandos:

```shell
composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
php temp/coding-standard/ecs check src
```

Notas en el código
---------------

Las notas no tienen ningún efecto sobre el procesamiento del código y son sólo para el uso del programador. Para piezas de código más grandes y completas, es importante escribir una nota que explique para qué sirve el código y cómo funciona en principio.

```php
// Definiciones de las variables
$a = 5;
$b = 3;
$c = 2;

// Suma de todos los números
$sum = $a + $b + $c;

// Listado de usuarios
echo $sum;
```

La nota comienza con un par de barras (`//`) y es válida hasta el final de la línea. Se puede utilizar en cualquier lugar.

La nota no debe explicar la aplicación concreta del algoritmo, sino sus principios generales. Esto se debe a que el código puede cambiar varias veces a lo largo del tiempo, en cuyo caso deberíamos corregir también la nota.

> **Nota del autor:**
>
> A menudo ocurre que el código no hace exactamente lo que explica su descripción. Esto se debe principalmente a que el programador comete un error en alguna parte. Por lo tanto, la nota debe describir los principios generales para que luego podamos modificar el código en consecuencia. Pero nunca olvides que la única verdad sobre lo que realmente ocurre en una aplicación la describe el código real, y la nota no tiene ningún efecto sobre él.

Separación gráfica de las partes del código
----------------------------

Al diseñar una aplicación, es importante separar los bloques lógicos entre sí. Por lo general, se separan en funciones, métodos o, en el caso del código básico, al menos mediante comentarios.

En un algoritmo más largo, suelo describir el principio completo del algoritmo al principio, y luego enumero los lugares individuales en el código para que el desarrollador pueda entender mejor la funcionalidad específica basada en ellos.

```php
/**
 * La función calcula la media aritmética.
 *
 * 1. Obtener una lista de números
 * 2. Obtener la suma y el número de números
 * 3. Calcular e imprimir la media
 */

// 1.
$numbers = [1, 3, 8, 12];

// 2.
$sum = array_sum($numbers);
$count = count($numbers);

// 3.
echo 'La media es:' . ($sum / $count);
```

Los caracteres `/**` inician un comentario de varias líneas que se aplica hasta el marcador `*/`. Para facilitar la lectura, es conveniente colocar un asterisco al principio de cada línea.

Comentarios sobre la documentación
----------------------

Los comentarios de la documentación suelen utilizarse para describir y documentar las funciones (comportamiento, parámetros, valores de retorno, autor, etc.).

En versiones anteriores de PHP (antes de la `7.0`), todavía no se utilizaban los tipos de datos, por lo que el tipo de una variable concreta se describía directamente en el comentario.

```php
/**
 * @autor Jan Barášek <jan@barasek.com>
 * @licencia MIT
 * @link https://php.baraja.cz
 * @param float[] $numbers
 */
function average(array $numbers): float
{
    $sum = array_sum($numbers);
    $count = count($numbers);

    return $sum / $count;
}
```

Los comentarios de la documentación se denominan "documentación" principalmente porque tienen un formato preacordado que entienden los entornos de desarrollo específicos (y los editores), pero también las herramientas automatizadas para generar documentación o comprobar el código.

¿Escribir el código en checo o en inglés?
-----------------------------

Escribo todo el código sólo en inglés (incluyendo los nombres de las funciones, las variables, los comentarios, ...).

Esto tiene una serie de ventajas:

- El promotor puede formar a su inglés de forma proactiva desde el primer momento.
- Una gran parte de la aplicación utiliza bibliotecas de terceros que están en inglés, por lo que mantiene automáticamente la coherencia
- La mayoría de las cosas avanzadas no tienen traducción al inglés
- Seguro que se te ocurren muchos otros ejemplos

PHP no requiere directamente el inglés y se puede escribir todo en inglés. Veo el uso del inglés más bien como una especie de inversión para el futuro, y una oportunidad de ampliar fácilmente el código por parte de otras personas que no son nativas del inglés.

El código completamente localizado en inglés también se utiliza en las empresas, por lo que es bueno practicar el inglés desde el principio.

Orden de las operaciones numéricas
------------------------

Tenga siempre en cuenta que PHP redondea los números cuando realiza operaciones numéricas. Esto puede ser a menudo una molestia, ya que cualquier resultado con números decimales va acompañado de cierta inexactitud.

Una buena solución parece ser incrementar primero los números y luego calcular con los números más grandes posibles. De este modo, hay menos distorsión estadística.

Ejemplo:

```php
echo 10 / 3; // Escribe 3,33333333333
```

En algunos casos, también se puede utilizar el truco de no usar decimales en absoluto y calcular todo como un número entero. En este caso, no hay tal distorsión:

```php
echo 1 / 2 * 2; // esto es peor porque 1/2 = 0,5*2 = 1
echo 2 * 1 / 2; // esto es mejor porque 2*1 = 2/2 = 1
```

Cuando resuelvas operaciones numéricas grandes y complejas, utiliza las fracciones para escribir los números.
