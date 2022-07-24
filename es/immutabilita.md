Inmutabilidad de los objetos: un concepto de diseño clave
=========================================================

> id: '057467db-4e3b-4e18-9ea5-dfb25feb3800'
> slug:
> 	cs: immutabilita
> 	es: inmutabilidad-de-los-objetos-un-concepto-de-diseno-clave
> 
> publicationDate: '2022-07-24 15:00:00'
> mainCategoryId: ae4c1c70-11b3-433e-b1d0-e590155bb8b9
> sourceContentHash: '88c26f35883c860426b4708f0be8761f'

La inmutabilidad es uno de los conceptos de diseño más importantes para construir aplicaciones estables. El principio básico establece que una vez que se escribe un estado, sólo puede leerse posteriormente sin posibilidad de modificarlo. Si necesitamos cambiar el estado, tenemos que crear una nueva instancia y sustituir todo el objeto por otro.

Por tanto, los tipos de datos pueden dividirse a grandes rasgos en dos grandes categorías:

- **Mutable** (estado mutable dentro de una única instancia)
- **Imutable** (estado interno inmutable)

Los objetos mutables pueden ser modificados internamente. Es decir, proporcionan operaciones que, al ser llamadas en diferentes combinaciones, hacen que obtengamos diferentes resultados. La inmutabilidad intenta evitar este comportamiento.

Definición
--------

> Una clase es **inmutable** precisamente si los datos de la instancia no pueden ser modificados de ninguna manera después de la creación de la instancia.

Así que todos los datos se fijan en el constructor. Todos los tipos de datos escalares también son automáticamente inmutables.

Una gran ventaja
--------------

Diseñar aplicaciones con estados inmutables proporciona una ventaja fundamental en la seguridad de la ejecución de las operaciones. Si sabemos que, una vez escritos, los datos no pueden ser modificados (mutados) posteriormente, podemos, por ejemplo, depurar de forma muy fiable, o dividir la aplicación en subfunciones sin riesgo de olvidar alguno de los estados intermedios.

La idea de inmutabilidad se opone generalmente al principio de almacenar estados en propiedades en objetos/entidades, y describe más bien un enfoque funcional en el que los datos simplemente "fluyen" a través de la aplicación, como hace javascript por ejemplo.

Desde el punto de vista del rendimiento, podemos decir automáticamente de los objetos inmutables que pueden ser almacenados en caché indefinidamente, porque nunca estarán desactualizados.

Un ejemplo real de PHP
--------------------

El uso más común de los objetos inmutables en PHP es el objeto `DateTimeImmutable`, que una vez creado sólo puede ser llamado por los métodos de formato. Si modificamos la configuración interna, el método devolverá una nueva instancia. Esta característica es crucial cuando se usa un ORM que utiliza el llamado patrón de identidad - nos permite, por ejemplo, garantizar que cuando leemos la fecha de creación de un pedido, ésta será la misma en todas partes de la aplicación y la integridad referencial no se verá corrompida.

Un ejemplo concreto de objeto mutable:

```php
$date = new DateTime('2021-05-14');
$tomorrow = $date->modify('+1 día');
echo $date->format('Y-m-d'); // 2021-05-15
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

Se imprimía la misma fecha porque el método `modify()` sólo cambiaba el estado interno del objeto `DateTime` y devolvía la misma instancia. Por lo tanto, no hubo la llamada **mutación del estado interno**, que es un comportamiento básico de la programación orientada a objetos. La actualización de la variable también cambió la original.

Y ahora un ejemplo de objeto inmutable:

```php
$date = new DateTimeImmutable('2021-05-14');
$tomorrow = $date->modify('+1 día');
echo $date->format('Y-m-d'); // 2021-05-14
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

El objeto `DateTimeImmutable` es inmutable, lo que significa que su estado interno nunca cambia. Una nueva instancia modificada (también inmutable) se almacena en la variable después de llamar al método `modify()`. Si no almacenamos el nuevo valor en la variable, no se podrá utilizar posteriormente.

El valor original no se toca nunca y permanece almacenado de forma segura.

¿Cuándo debe ser inmutable una clase?
---------------------------

A menos que tengas una muy buena razón para hacerlo mutable, escribe siempre una clase o función como inmutable. Esto simplificará su diseño en el futuro.

Las clases mutables deben cambiar lo menos posible. Siempre recomiendo documentar el comportamiento de la inmutabilidad.

Quizá el único inconveniente de la inmutabilidad es que hay que crear una nueva instancia con cada cambio de atributo, lo que tiene un impacto menor en el rendimiento. Si tu aplicación (como la mayoría de las aplicaciones) suele mostrar datos y cambiarlos con menos frecuencia, esta desventaja es bastante insignificante con el rendimiento de los ordenadores actuales.

¿Qué tipos de datos deben ser inmutables?
------------------------------------

- Todos los identificadores y códigos únicos
- La mayoría de las sesiones de bases de datos ManyToOne y OneToOne
- Fechas, horas, valores del calendario
- Recorrido por arrays en los que queremos hacer la misma operación en cada elemento
- Intervalos, pares, triples, ...
- Figuras geométricas, puntos, líneas, coordenadas GPS, direcciones físicas, ...
- Registros y fichas históricas
- Información sobre los pedidos procesados y la mayoría de los datos financieros
- Metadatos sobre la entidad relacionada

Lo que no debe ser inmutable:

- Objetos grandes con muchas propiedades
- La mayoría de los resultados tabulares de una base de datos, como las entidades de Doctrine
- Construir progresivamente objetos a partir de piezas más pequeñas
