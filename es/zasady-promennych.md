Principios de redacción de variables
====================================

> id: '4c8e631b-b305-4169-8885-4f9506155999'
> slug:
> 	cs: zasady-promennych
> 	es: principios-de-redaccion-de-variables
> 
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '5528d9b73c1d1c07c330a58e4aeaa06a'

Esta es la segunda parte de una serie de tutoriales sobre PHP. En este episodio, veremos las reglas básicas para la escritura de variables.

Esta página es sólo un resumen rápido. Si buscas una descripción técnica detallada de todas las características, he escrito un <a href="/variable">artículo aparte</a>.

Sintaxis básica
--------------------------

Las variables en PHP comienzan con el signo de dólar `$` seguido inmediatamente por el nombre.

```php
$zvire = 'cat';
```

Las cadenas (secuencias de caracteres) van entre comillas o apóstrofes:

```php
$a = "Comillas";
$b = 'apóstrofes';
```

Los dígitos no van entre comillas:

```php
$a = 5;
$b = 10;
$c = 3.14159;
```

El nombre de la variable sólo puede estar formado por caracteres del alfabeto inglés y números. El nombre siempre empieza con una letra.

Si el nombre se compone de más de una palabra, es habitual utilizar la sintaxis `camelCase` (la primera letra en minúscula y todas las demás palabras comienzan con mayúscula):

```php
$kocka = 'Kitty';
$rychlyPocitac = '¡Claro que es mío!';
$pocetRohuJednorozce = 1;
```

El nombre no debe contener espacios, guiones, ganchos, comas, comillas, paréntesis u otros caracteres especiales. El único carácter especial permitido es el `subrayado`.

Los números decimales se escribirán con un punto:

```php
$pi = 3.14159;
```

A menudo puede ser útil realizar operaciones matemáticas directamente al definir una variable:

```php
$a = 5;
$b = 3;
$c = $a + $b;	// sumar 5 + 3
echo $c;		// imprime 8
```

Inserción correcta de una comilla o un apóstrofe
--------------------------

Las comillas y los apóstrofes no deben combinarse arbitrariamente. Por ejemplo, si decidimos usar una comilla, también debemos terminar la cadena con la comilla y no usarla dentro.

Por lo tanto, esto es un error:

```php
echo "<img src="obrazek.gif">";
```

Porque no está claro dónde empieza y termina la cadena. Las comillas y los apóstrofes no se pueden anidar.

Una posible solución se llama <a href="/escapovani">escapar</a>, donde el carácter problemático va precedido de una barra invertida.

```php
echo "<img src="image.gif">";
```

La barra invertida indica que el siguiente carácter será exactamente el que queremos utilizar.

Sin embargo, para la salida del código HTML, es preferible encerrar toda la cadena entre apóstrofes y luego utilizar las comillas de forma normal:

```php
echo '<img src="image.gif">';
```

También se puede invertir:

```php
echo "<img src='picture.gif'>";
```

Rellenar una variable desde una dirección url o desde un formulario
--------------------------

Las direcciones que contienen un signo de interrogación llevan información sobre las variables de entrada, así por ejemplo `index.php?page=contacts` denota la variable `page` con el valor `contacts`. El valor de esta variable se lee como `$_GET['page']`.

El carácter de signo de interrogación no tiene ninguna relación con el nombre del archivo en el disco. Siempre es el mismo archivo al que le pasamos parámetros en la dirección.

Este tema lo trato con detalle en mi artículo sobre <a href="/methods-odesilani-dat">métodos de envío de datos</a>.

Definir el contenido de una variable a partir de una dirección
--------------------------

Algunas variables están disponibles en el momento en que se ejecuta el script (y por lo tanto se pueden utilizar de inmediato), estas se llaman **variables superglobales**. Por ejemplo, si queremos leer un valor de una URL, utilizamos la variable `$_GET`.
El uso es el siguiente:

```php
$a = $_GET['a'];

echo $a;
```

Este script imprime lo que tiene en la URL después del signo de interrogación en el código fuente.

> ¡Atención, esta muestra no es segura! Si un visitante malintencionado pasara, por ejemplo, un código HTML en la URL, éste se insertaría en la página y se ejecutaría. Por lo tanto, debemos tratar siempre la salida; para ello se utiliza la función `htmlspecialchars()`.

```php
$a = $_GET['a'];

echo htmlspecialchars($a);
```

> Si accedemos a la página sin especificar el parámetro `?a=cualquier cosa`, la variable `$_GET['a']` no existirá y PHP lanzará un mensaje de error. Necesitamos tratar esta condición con una condición y no hacer nada si la variable no existe (o alternativamente, dar salida a un contenido alternativo). La existencia de la variable puede ser verificada con la función `isset()`.

```php
if (isset($_GET['a'])) {
    $a = $_GET['a'];

    echo htmlspecialchars($a);
} else {
    echo 'La variable "a" no existe.';
}
```

Ejemplo con recuento
--------------------------

Con las variables de la dirección URL podemos hacer perrerías, por ejemplo, sumarlas y escribir el resultado directamente:

```php
echo $_GET['a'] + $_GET['b'];
```

Si queremos incluir más parámetros de entrada en la URL, debemos separarlos con un ampersand (`&`). La dirección puede tener el siguiente aspecto: `index.php?a=5&b=3`.

Vinculación de entradas de texto (cadenas)
--------------------------

También podemos vincular fácilmente 2 entradas de texto (cadenas). Para ello se utiliza el operador punto. Se puede enlazar en una variable o al hacer un listado.

```php
$a = 'perro';
$b = 'cat';

echo $a . 'a' . $b;
```

Dice "perro y gato".
