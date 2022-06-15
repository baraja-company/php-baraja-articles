Tabla de contingencia en PHP
============================

> id: '9bdc1004-8f06-48ec-8f56-8707fad5cef7'
> slug:
> 	cs: kontingencni-tabulka
> 	es: tabla-de-contingencia-en-php
> 
> publicationDate: '2019-11-13 22:00:05'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '80fdc1436cd30bc39ffe9c11c3d86c41'

Una tabla de contingencia se utiliza generalmente para mostrar la relación entre dos fenómenos estadísticos. Al desarrollar una aplicación web, a menudo necesitaremos visualizar la relación de un determinado fenómeno en la base de datos con una secuencia temporal, normalmente en la administración.

Por ejemplo, tenemos una tabla de pedidos que muestra productos individuales y nos interesa saber cómo se relacionan las ventas de ciertos productos de gran volumen con el tiempo.

Para ello, sería útil una tabla como la siguiente:

| Dátiles, manzanas, fresas, peras, etc.
|---------|--------|--------|--------|
| 2019-05 | 10 | 15 | 18 |
| 2019-04 | 12 | 18 | 11 |
| 2019-03 | 13 | 9 | 21 |
| 2019-02 | 6 | 17 | 10 |
| 2019-01 | 7 | 4 | 6 |

No hay una manera fácil de preparar los datos en este formulario en PHP, y obtenerlos directamente en este formulario directamente en SQL tampoco es elegante, porque tenemos que tener en cuenta que hay un número dinámico de columnas.

Así que tenemos que ser inteligentes al diseñar la salida de esta estructura de datos.

Serialización de datos mediante claves
----------------------------

Cuando construyo una tabla, a menudo utilizo la recuperación de todos los registros que cumplen una condición determinada directamente desde la base de datos, por ejemplo, datos de intervalo.

Específicamente:

```sql
SELECT *
FROM `order`
WHERE `inserted_date` <= '2019-05-01'
ORDER BY `inserted_date` DESC
```

La consulta recupera todas las columnas de la tabla de pedidos (`order`), filtrando todos los registros desde el principio de las edades hasta el ``2019-05-01``, devolviendo ordenados de más reciente a más antiguo.

Con una simple consulta SQL, obtenemos los datos casi al instante. La segunda característica interesante es que los índices de la base de datos pueden utilizarse de forma eficiente en la compilación de los resultados. Sin embargo, como tenemos los datos en un array plano, debemos serializarlos manualmente en una estructura de datos que pueda convertirse en una tabla contig.

Dado que una tabla de contingencia describe la relación de dos o más factores, tiene sentido utilizar una clave multidimensional. Sin embargo, dado que algunos datos pueden no existir para todas las combinaciones, es mejor serializar la clave a una sola cadena y almacenar los datos como una matriz plana.

Los datos se pueden ensamblar en una sola pasada del bucle (la variable `$selection` contiene la salida de la base de datos):

```php
$data = [];

foreach ($selection as $row) {
    $date = date('Y-m', $row->insertedDate); // Fecha año-mes
    foreach ($row->items as $product) { // pasamos por los productos
        $key = $date . '_' . $product->id;
        if (isset($data[$key])) {
            $data[$key]++; // existe, añadiremos otro producto
        } else {
            $data[$key] = 1; // no existe, empezaremos con el primer producto
        }
    }
}
```

Si estuviéramos explorando una estructura de datos más sencilla, no sería necesario un bucle interno para recorrer los productos. En este caso, toda la construcción de datos podría resolverse con un solo ciclo.

Con este enfoque, obtenemos un llamado array plano de valores que se parece a un `clave: valor', mientras que almacena información bidimensional.

La salida es entonces, por ejemplo (en `Mayo 2019`, un producto con ID `10` vendió `6` unidades):

```php
$data = [
    '2019-05_10' => 6,
    ...
];
```

Renderizar datos en una tabla - plantillas
-------------------------------

Si tenemos los datos en forma de array plano, podemos renderizar toda la tabla muy fácilmente. Para ello, sólo necesitamos conocer los campos de todos los productos que nos interesan y los campos de todas las fechas para las que queremos trazar la tabla.

```php
$products = [ ... ]; // campo del producto: id => nombre
$dates = [ ... ]; // por fecha: fecha => etiqueta

echo '<table>';
foreach ($products as $productId => $productName) {
    echo '<tr>';
    foreach ($dates as $date => $dateLabel) {
        echo '<td>';
        echo htmlspecialchars(
            (string) ($data[$date . '_' . $productId] ?? '0')
        );
        echo '<td>';
    }
    echo '</tr>';
}
echo '</tabla>';
```

Tenga en cuenta que al navegar por los datos, busca una ocurrencia específica por el plegado de la clave de la cadena. Este enfoque nos permite restringir o ampliar la tabla renderizada de forma arbitraria en función de los datos que estemos consultando. Si los datos no existen, se evalúa el operador ternario `??` y se muestra el cero.

Podemos construir matrices de productos y fechas disponibles como parte del primer ciclo que prepara los datos. En ese momento, estaremos seguros de que sólo estamos trazando los datos que realmente existen. En este caso, es muy importante que la salida de la base de datos SQL esté ordenada por fecha de creación, ya que de lo contrario las filas pueden ser barajadas durante el renderizado final de la tabla.
