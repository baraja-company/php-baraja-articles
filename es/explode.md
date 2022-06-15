Función PHP Explode - dividir cadena por separador
==================================================

> id: '4dc7cec6-0e96-4a6b-aee8-32c8817ba11e'
> slug:
> 	cs: explode
> 	es: funcion-php-explode-dividir-cadena-por-separador
> 
> perex:
> 	- Rozdělení řetězce na více částí podle oddělovače.
> 	- Dividir la cadena en varias partes según el separador.
> 
> publicationDate: '2019-11-26 11:39:36'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '66725772be4aae0df8af399e7ce2ba07'

Explotar se utiliza para dividir fácilmente una cadena por un separador.

- Devuelve los resultados individuales como una matriz numerada desde cero,
- no se puede insertar un array, sólo se introduce la cadena,
- no puede cambiar el delimitador durante el análisis sintáctico, no puede seleccionar varios delimitadores.

| Soporte de PHP 4 y posterior
|---------------|-----------------|
| Breve descripción | Dividir una cadena en un array por separador.
| Requisitos: Ninguno
| Nota | No se puede insertar un array, sólo una cadena.

A menudo necesitamos dividir una cadena según alguna regla sencilla. Por ejemplo, una lista de números separados por comas.

La función explode() es genial para esto, ya que toma el separador (que separa la cadena) como primer parámetro y los datos en sí como segundo parámetro:

```php
$cisla = '3, 1, 4, 1, 5, 9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
	echo $cislo . '<br>';
	// Aquí podemos procesar más los números
}
```

¿Pero qué pasa si los números están separados por comas, pero hay espacios alrededor?

La solución es analizar por la subcadena común más pequeña y luego eliminar los caracteres no deseados a su alrededor (espacios y otros espacios en blanco):

```php
$cisla = '3, 1,4, 1 , 5 ,9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
     echo trim($cislo) . '<br>';
     // Aquí podemos procesar más los números
}
```

En este caso, la función `trim()` elimina elegantemente los espacios en blanco alrededor de los caracteres (espacios, tabulaciones, saltos de línea, ...), dando sólo datos limpios.

Límite, que limita el número de entidades analizadas en una matriz
--------------------------

> CONSEJO: Para muchos ejemplos, explode() no es adecuado y es mucho mejor utilizar expresiones regulares.

Sin embargo, a menudo sólo queremos analizar los datos hasta una determinada distancia, y el tercer parámetro (opcional) límite se puede utilizar para este propósito.

Por ejemplo, tengamos datos estructurados separados por dos puntos en los que queremos obtener el contenido después de los primeros dos puntos e ignorar los demás.
Ejemplo:

```php
$cas = 'formato: "j.n.Y - H:i"';
```

Si tuviéramos que analizar la cadena sólo como:

```php
$parser = explode(':', $cas);
```

Obtendríamos estas 3 subcadenas (en otros casos podría haber muchas más):

```php
'formato'
'"j.n.Y - H'
'i"'
```

Por lo tanto, establecemos un límite a la cantidad de elementos a obtener (y posiblemente todos los demás se añadirán al final del último elemento):

```php
$parser = explode(':', $cas, 2);

// Vuelve:
echo $parser[0]; // formato
echo $parser[1]; // "j.n.Y - H:i"
```

> **Nota:** Las comillas no deseadas pueden eliminarse de la cadena, por ejemplo, utilizando la función `trim()`:

```php
echo trim($parser[1], '"'); // el segundo parámetro especifica el mapa de caracteres a eliminar
```

Between, obtener una cadena entre dos cadenas
--------------------------

A menudo necesitamos recuperar una cadena que está delimitada por otras dos cadenas. No hay una función para esto directamente en PHP, pero podemos escribirla nosotros mismos:

```php
function between(string $left, string $right, string $data): string
{
   $l = explode($left, $data);
   $r = explode($right, $l[1]);

   return $r[0];
}
```

Expresiones regulares
--------------------------

Se puede lograr una división mucho más elegante y trabajar con cadenas utilizando <a href="/regex">expresiones regulares</a>, de las que hablo en una página aparte.

Funciones similares
--------------------------

- <a href="/function-implode">Implode()</a> - concatenar una matriz en una cadena

Ejemplo de análisis de direcciones IP
--------------------------

```php
$ip = '10.0.0.138';
$parser = explode('.', $ip);
echo $parser[0]; // imprime "10"
echo $parser[1]; // imprime "0";
```

La variable `$ip` contiene una cadena de entrada que se analiza según el delimitador `.`, el retorno es un array. El análisis continúa hasta el final de la cadena a menos que se especifique un límite.

Parámetros
--------------------------

| # | Tipo | Descripción
|---|--------|------|
| 1 | cadena | Cadena de separación.
| 2. Cadena. Cadena analizada.
| 3. Límite de análisis. Este es un parámetro opcional. Ejemplo:

```php
$text = 'PHP es mi lenguaje favorito.';
$parser = explode('', $text, 1);

echo $parser[0]; // imprime la primera palabra
echo $parser[1]; // imprime el resto del texto
echo $parser[2]; // ¡no sale nada porque se ha establecido un límite!
```

Valores de retorno
--------------------------

El valor de retorno es un array con una cadena analizada.

Los índices se numeran de cero a `X` a menos que se especifique un límite.

Diferencias con las versiones anteriores
--------------------------

| Versión de PHP Descripción
|-----------|-------|
| 5.1.0 | Se ha añadido soporte para el límite negativo en los pases.
| Se ha añadido el parámetro opcional **limit**.

Consejos y notas
--------------------------

Cuando se utiliza un **límite** negativo, se da el número de elementos desde el final de la cadena.

Ejemplo:

```php
$str = 'uno|dos|tres|cuatro';

// límite positivo
print_r(explode('|', $str, 2));

// límite negativo (desde PHP 5.1)
print_r(explode('|', $str, -1));
```

Se devolverán los siguientes campos:

```php
Array
(
    [0] => one
    [1] => two|three|four
)

Array
(
    [0] => one
    [1] => two
    [2] => three
)
```
