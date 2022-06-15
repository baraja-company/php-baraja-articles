Variables en PHP
================

> id: b89774e7-143c-4a8c-8dc6-3b3d2c78d5b7
> slug:
> 	cs: promenna
> 	es: variables-en-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '23e4d537f18a3b2e1946c79be1aacf52'

Esta página es un resumen completo de cómo funcionan las variables en PHP. El texto está redactado en un estilo ligeramente técnico y puede no ser comprendido del todo por los principiantes. Si te interesan los fundamentos completos, entonces lee <a href="/primer-script">el tutorial para principiantes</a> y <a href="/principios-del-script">principios de escritura de variables</a>.

Descripción
-----

Una variable es una ubicación virtual en la memoria operativa, está definida por `nombre` y `tipo de datos`. Dentro del tipo de datos, la variable tiene entonces un "contenido".

Internamente, PHP representa las variables como una llamada tabla hash, es decir, todas las variables se almacenan en una tabla que es muy rápida de buscar, por lo que el tiempo requerido para acceder a cada variable es *casi* constante.

Ejemplos de escritura:

```php
$a = 10;
$b = 'cat';
$c = true;
```

Cada línea de la muestra denota la definición de una variable. El nombre de cada variable comienza con el signo de dólar `$` seguido del propio nombre. El signo de igualdad puede utilizarse para asignar un valor a la variable.

Internamente, las variables se almacenarán en memoria como una tabla hash:

| Nombre, tipo, abreviatura, valor, etc.
|-------|---------|---------|---------|
| `$a` | integer | int | `10` |
| `$b` | cadena | cadena | `cats` |
| `$c` | boolean | bool | `true` |

Tipos de variables
---------------

Las variables se clasifican según los derechos de acceso y el uso:

- <a href="/variable-local">variables locales</a> (válidas dentro del contexto, es decir, dentro de funciones y métodos),
- <a href="/variable-global">variables globales</a> (válidas dentro de todo el script),
- <a href="/variable-superglobal">Variables superglobales</a> (válidas en todo el entorno del servidor - típicamente datos del usuario),
- <a href="/promenna-variable">variable variable</a> (una variable especial que contiene una referencia (enlace) a otra variable).

* Las variables globales y las variables variables no deben utilizarse porque contribuyen a la ilegibilidad del código y al comportamiento "mágico" (inesperado) de la aplicación.

Contenido permitido de las variables
--------------------------

Las variables pueden contener cualquier cosa que permita su tipo de datos actual. Si no se especifica el tipo de datos, se determinará automáticamente, basándose en el contenido (esto no se recomienda, ya que puede dar lugar a errores).

Los tipos de datos funcionan de forma independiente, por lo que podemos utilizar casi cualquier tipo. Sin embargo, si queremos realizar alguna operación de fusión, debemos asegurar siempre la conversión a un formato.

Un buen ejemplo es, por ejemplo, la suma y la multiplicación de números:

```php
$x = 5;       // entero
$y = 3;       // entero
$z = $y + $y; // la variable $z se compondrá en base a múltiples variables
```

En este caso, PHP se enfrenta a la pregunta de qué tipo de datos tendrá la variable recién creada `$z`. Si son del mismo tipo de datos y la operación es posible, se hereda el tipo de datos.

Sin embargo, a veces podemos realizar una operación con varios tipos de datos:

```php
$x = 1;       // entero
$y = 3.14159; // flotante
$z = $y + $y; // flotante
```

En este caso fusionamos entero y flotante. La salida será un número decimal, por lo que se utiliza float. En este caso, PHP hará algo llamado **repartición dinámica**.

Sin embargo, no siempre podemos confiar en este comportamiento. Por ejemplo, ¿cómo te gustaría fusionar un número y una cadena?

```php
$x = 256;     // entero
$y = '¡Oye! ¡Oye!'; // flotante
$z = $y + $y; // ???
```

Tipos de datos (resumen de los más importantes)
--------------------------------------

PHP es un lenguaje interpretado, por lo que tiene algunas peculiaridades en comparación con otros lenguajes de programación, una de ellas es que no tenemos que especificar tipos de datos para las variables, es decir, cada variable cambia dinámicamente su tipo de datos según su contenido (a menos que se indique lo contrario). Aun así, es bueno conocer al menos los tipos de datos básicos, especialmente útiles cuando se optimizan las aplicaciones o se trabaja con bases de datos.

La notación puede ser así:

```php
$x = (int) 25; // crea una variable de tipo entero
```

<a href="/datove-typy">Resumen de tipos de datos</a>.

Herencia de tipos de datos
-----------------------

¿Qué tipo de datos tendrá la variable `$x` si sólo conocemos este trozo de código fuente?

```php
$x = $y;
```

Depende del tipo de datos de la variable `$y` de la que se heredará tanto el valor como su tipo de datos. En este caso, no conocemos la variable `$x`, por lo que no podemos seguir evaluando el código y se lanzaría un mensaje de error.

Anulación dinámica
---------------------

Tenemos las siguientes 2 variables:

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

En el segundo caso, se produce la llamada **sobreescritura dinámica**, es decir, la variable convierte su tipo de datos para que se pueda realizar una operación computacional sobre ella. No siempre se puede confiar en este comportamiento, y es más bien un comportamiento correctivo para arreglar scripts de principiantes mal escritos. Si es posible, escriba siempre los números con un tipo de datos para almacenarlos, ya que esto aumenta su precisión y facilita su uso en el futuro.

> **Nota:** Es importante tener en cuenta que no podemos convertir los tipos de datos de forma completamente arbitraria, por lo que esto no siempre es posible. Si se sobrescribe un tipo de datos a otro (incompatible), puede que la conversión no se produzca en absoluto, o que el contenido original se corrompa o se destruya por completo y se sustituya por otro. Por ejemplo, si se reescribe una cadena a un número entero (y se almacena en la variable algún texto que no sea un número), se almacenará en la variable el valor `1` en lugar del valor numérico.

Representar cadenas como matrices
------------------------------

Todas las cadenas se almacenan internamente como una matriz de caracteres. Es decir, cada personaje tiene su propio **índice** y puede ser referenciado. Si no especificamos un índice, se trabaja con toda la cadena.

```php
$x = '¡Programemos en PHP!';
$n = 3;

echo $x;		// esto imprime todo el contenido de la variable $x
echo $x[0];		// esto imprime el carácter nulo de la variable $x
echo $x[$n];	// esto imprime el enésimo carácter de la variable $x
```

> **Nota:** Números PHP a partir de cero, es decir, el carácter cero es 'P' y el primer carácter es 'r'.
>
> > Además, los caracteres son conmutados por bytes. Por ejemplo, el carácter "no" en la codificación UTF-8 tiene una longitud de 2 bytes, por lo que el índice del carácter en la cadena no coincidirá con la posición real al desplazarse y se utilizarán 2 índices para almacenar el carácter.

La existencia de un elemento de un array debe verificarse siempre con la función `isset()`:

```php
if (isset($x[$n])) {
    echo $x[$n];
}
```

Alternativamente, puedes escribirlo bien con un operador ternario:

```php
echo $x[$n] ?? '';
```

Copia de variables
---------------------

Tengamos la siguiente variable:

```php
$q = 'Lorem ipsum, ...';
```

Y luego copiar su valor a la siguiente variable:

```php
$qi = $q;
```

Afortunadamente, no se copiará nada y PHP se limitará a almacenar una referencia al valor en una **tabla hash**. El valor sólo se copiará realmente cuando el valor de una de las variables deba cambiar. Este comportamiento es manejado por un componente generalmente llamado **Colector Garbridge**.
