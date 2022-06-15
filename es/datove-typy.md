Tipos de datos en PHP
=====================

> id: cd63818c-259a-484e-b006-86c35b362019
> slug:
> 	cs: datove-typy
> 	es: tipos-de-datos-en-php
> 
> perex:
> 	- 'Datové typy v PHP: String, integer, float, boolean, pole (array), objekt a další.'
> 	- 'Tipos de datos en PHP: string, integer, float, boolean, array, object y más.'
> 
> publicationDate: '2019-08-23 15:44:28'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: fcb2c647973c5f94e601c789b0a882f1

Todos los datos procesados en PHP son de un tipo determinado. Por ejemplo, un entero, una cadena o un booleano (verdadero/falso).

Tipos de datos básicos
--------------------

Los tipos básicos también se llaman **tipos de datos primitivos**, o <a href="/función-es-escalar">tipos escalares</a>.

| Tipo, nombre, descripción, etc.
|---------|-----------------|-------|
| `int` | Integer | (`integer`) Contiene sólo enteros. El valor máximo está determinado por el número de bits. En el PHP de 32 bits el rango va de `-2.147.483.648` a `-2.147.483.647` (~ ± 2 mil millones), el PHP de 64 bits tiene un rango de `-9.223.372.036.854.775.808` a `-9.223.372.036.854.775.807` (~ ± 9 quintillones). El valor máximo se puede obtener siempre llamando a la constante `PHP_INT_MAX`. Si se supera el valor máximo de un entero, PHP representará el número como `float` y lo sobrescribirá automáticamente.
| `float` | Decimal de punto flotante | Es una variante de número de punto flotante para la que se aplica la regla *"cuanto más pequeño, más preciso "*. El número se almacena internamente como una **mantisa** y un **exponente**, por lo que en realidad se almacenan 2 números entre los que se realiza la operación `mantisa * (2^exponente)`, lo que permite almacenar un rango de números realmente enorme. Esto utiliza el principio de que para los números grandes no siempre necesitamos saber su valor exacto, pero queremos ahorrar tanta memoria como sea posible. Los números del tipo `float` no necesitan ser almacenados con exactitud y no deben ser utilizados para calcular dinero.
| `string` | String | Contiene una secuencia de caracteres delimitada por comillas o apóstrofes. La longitud máxima está limitada únicamente por la capacidad de la memoria operativa. Una cadena puede ser almacenada en cualquier codificación, contener emoji o datos binarios.
| (`boolean`) Valor booleano del álgebra booleana, sólo puede contener `true` o `false`.
| El valor vacío `null` es útil para los casos en los que queremos expresar que algo no existe. Por ejemplo, un artículo no tiene categoría. A veces `null` se sustituye incorrectamente por null (`0`) o cadenas vacías (`''`), pero esta no es una buena solución para expresar la inexistencia.

**Atención:** El tipo `null` no es escalar.

Cero (0) contra nulo
----------------

A muchos desarrolladores les cuesta entender la diferencia entre `0` (cero) y `null` (un valor inexistente) cuando empiezan a desarrollar.

Esta distinción se puede explicar muy bien y con humor con la siguiente imagen:

<img src="{$baseUrl}/images/0-vs-null.jpg" alt="0 vs. null" class="w-100 mb-3">

Reembalaje manual
--------------------

Algunos tipos pueden convertirse entre sí. El tipo de datos de destino se especifica mediante corchetes y puede especificarse en cualquier parte de la aplicación, por ejemplo al enumerar un valor.

Por ejemplo:

```php
$pi = 3.14;

echo $pi;       // imprime 3.14

echo (int) $pi; // imprime 3
```

Relleno dinámico
---------------------

Considera las siguientes 2 variables:

```php
$x = 10;
$y = '10';
```

¿Cuál es la diferencia entre el contenido de las variables `$x` y `$y`?

La variable `$x` es un número, `$y` es una cadena (que contiene un "1" y un "0"), por lo que si sólo almacenamos la variable en memoria y no realizamos ninguna operación que afecte al valor. Por ejemplo, las siguientes 2 entradas devolverán el mismo resultado:

```php
echo $x + 5;	// imprime 15
echo $y + 5;	// imprime 15
```

En el segundo caso, se produce la llamada **sobreescritura dinámica**, es decir, la variable convierte su tipo de datos para que se pueda realizar una operación computacional sobre ella. No siempre se puede confiar en este comportamiento y es más bien un comportamiento correctivo para arreglar scripts de principiantes mal escritos. Si es posible, escriba siempre los números con un tipo de datos para almacenarlos, ya que esto aumenta su precisión y facilita su uso en el futuro.

> **Nota:** Es importante tener en cuenta que no podemos convertir los tipos de datos de forma completamente arbitraria, por lo que esto no siempre es posible. Si sobrescribes un tipo de datos a otro (incompatible), puede que la conversión no se produzca en absoluto, o que el contenido original se corrompa o se destruya por completo y se sustituya por otro. Por ejemplo, si se reescribe una cadena a un número entero (y se almacena en la variable algún texto que no sea un número), se almacenará en la variable el valor `1` en lugar del valor numérico.

Comparación de tipos
----------------

Al comparar valores, hay que tener en cuenta los diferentes tipos de datos.

En general, el operador `==` se utiliza para la comparación general de dos valores (independientemente de los tipos), mientras que `===` compara tanto los valores como el tipo de datos.

Por ejemplo:

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
