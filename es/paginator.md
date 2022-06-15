Paginador y paginación de resultados en PHP
===========================================

> id: a1450160-e320-414a-8266-128465632e94
> slug:
> 	cs: paginator
> 	es: paginador-y-paginacion-de-resultados-en-php
> 
> perex:
> 	- Stránkování dlouhého seznamu položek. Jak řešit omezení počtu vypsaných položek a vypočítat stránkování.
> 	- Paginación de una larga lista de elementos. Cómo resolver la limitación del número de elementos listados y calcular la paginación.
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: d0bc71ded032401875396b0fd263a820

Cuando tenemos muchos datos que volcar, es conveniente dividirlos en varias páginas. Este artículo no aborda la implementación práctica de pasar números de página y listar resultados, sólo la extracción teórica de valores y el cálculo del libro de códigos óptimo para que la navegación por un gran número de páginas sea lo más fácil posible.

¿Cuántos resultados tenemos?
----------------------

Para empezar, tenemos que averiguar cuántos resultados tenemos en absoluto. Si los datos proceden de una base de datos, pueden contarse muy eficazmente con la siguiente sentencia SQL:

```sql
SELECT COUNT(*) FROM tabulka
```

El cálculo es muy rápido porque la base de datos guarda las estadísticas en un archivo de ayuda, por lo que no toca los datos en absoluto.

Si los datos provienen de otro lugar (y los tenemos en un array, por ejemplo), se pueden contar con la función count():

```php
$cisla = [3, 1, 4, 1, 5, 9, 2];

echo 'El campo contiene' . count($cisla) . 'números.';
```

Limitar el número de resultados
----------------------

Otro problema es la limitación del número de resultados. Si los datos están en la base de datos, basta con poner el parámetro `LIMIT` en la sentencia SQL:

```sql
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10
```

Este comando siempre obtendrá un máximo de 10 resultados, y también hará que la consulta sea más rápida porque la base de datos no tendrá que recorrer todos los archivos de datos.

Si tenemos datos de otra fuente (de nuevo un array), también podemos limitar los resultados a nivel de PHP utilizando la variable de ayuda `$iterator`:

```php
$pole = [...];

$iterator = 0;
$limit = 10;
foreach ($pole as $prvek) {
	// aquí es donde se vuelcan los datos

	$iterator++;
	if ($iterator >= $limit) {
	    break; // Detiene el ciclo cuando se ha ejecutado 10 veces
	}
}
```

Omitir los primeros resultados X
----------------------

Cuando estamos en la primera página, es bastante sencillo, sólo hay que limitar el número de resultados utilizando `LIMIT`. ¿Pero qué pasa si estoy en la tercera página? Entonces tenemos que omitir los primeros resultados `X`.

En SQL tenemos una notación elegante para esto de nuevo:

```sql
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10 OFFSET 20
```

Se salta los primeros 20 resultados y limita la siguiente salida a 10 resultados, por lo que se obtiene el intervalo `<21 - 30>`.

En PHP puro esto se maneja de dos maneras.

Si conocemos los índices del array, podemos empezar a leerlo a partir de un punto determinado (lo que es muy rápido):

```php
$pole = [...];

$start = 20;
$limit = 10;
for ($i = $start; ($i <= $start + $limit && isset($pole[$i])); $i++) {
	// aquí es donde se vuelcan los datos
}
```

Sin embargo, para un campo desconocido tenemos que utilizar de nuevo el iterador y saltar los elementos:

```php
$pole = [...];

$iterator = 0;
$start = 20;
$limit = 10;
foreach ($pole as $prvek) {
	if ($iterator < $start) {
		$iterator++;
		continue;
	}

	// de alguna manera los datos se están volcando aquí

	$iterator++;
	if ($iterator >= $start + $limit) break;
}
```

Visualización del paginador/escalón óptimo
----------------------

Supongamos que conocemos el número total de elementos, el número de elementos de la página y el número de página actual. Ahora queremos hacer una barra que permita navegar rápidamente por todas las páginas con resultados de búsqueda. Sin embargo, como hay muchas páginas (del orden de miles), no podemos enumerarlas todas a la vez, así que tenemos que elegir inteligentemente algunas representativas que representen mejor el rango entre páginas.

Puede tener este aspecto:

```php
1 | 15 | 30 | 36 | 45 | 60 | 72
```

Asignación:

Estoy en la página 36 de 72, ¿cómo colocar óptimamente los números de página?
Bueno, a través de la secuencia.

> **Consejo:** Por observación práctica descubrí que la parte izquierda del Paginador debe ser calculada a través de una secuencia aritmética (así puedo moverme linealmente por el mismo número de pasos) y la parte derecha a través de una **secuencia geométrica**, lo que a su vez facilita la realización de un paso grande. Así, si quiero llegar a una página concreta, primero me salto un gran número de elementos innecesarios y luego afino la selección volviendo a la izquierda.

Teoría de la secuencia aritmética (seguimos sumando el mismo número):

```php
$d = 10;   // tamaño del paso
$a[1] = 1; // primer elemento
$a[2] = $a[1] + $d; // segundo elemento
$a[3] = $a[1] + 2 * $d;
$a[3] = $a[2] + $d;
$a[$n] = $a[1] + ($n - 1) * $d; // Enésimo elemento

function getAritmeticItem(int $start, int $step, int $n): int
{
	return $start + ($n - 1) * $step;
}
```

Teoría de la secuencia geométrica (siempre se multiplica por el mismo número):

```php
$q = 10;   // tamaño del paso
$a[1] = 1; // primer elemento
$a[2] = $a[1] * $q; // segundo elemento
$a[3] = $a[1] * $q * $q;
$a[3] = $a[1] * pow($q, 2);
$a[3] = $a[2] * $q;
$a[$n] = $a[1] * pow($q, $n - 1); // Enésimo elemento

function getGeometricItem(int $start, int $step, int $q): int
{
	return $start * pow($q, $step - 1);
}
```



```php
$start = 1;
$current = 36;
$end = 72;
```
