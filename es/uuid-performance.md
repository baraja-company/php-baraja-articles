UUID y el rendimiento de las aplicaciones a gran escala
=======================================================

> id: '2f072ce8-13b1-41f6-b328-2bd3b416cdd2'
> slug:
> 	cs: uuid-performance
> 	es: uuid-y-el-rendimiento-de-las-aplicaciones-a-gran-escala
> 
> publicationDate: '2019-11-08 10:09:54'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3b6d0e37684aedc0aedd92f2654e3626'

Cuando el tamaño de la base de datos crece más allá de millones de filas, es aconsejable empezar a escalar la aplicación y dividir la base de datos en varios servidores físicos.

El mayor problema de dividir la base de datos en varias partes es su posterior sincronización si el usuario solicita datos específicos.

Por qué utilizar UUID y cuáles son sus ventajas sobre el autoincremento
--------------------------------------------------------

Suponga que tiene una tabla de "artículos", pero como tiene un sitio enorme, hay decenas de millones de artículos más arriba y tiene que dividirlos físicamente en varias máquinas.

Si utilizáramos un entero ordinario como `id` (clave primaria) con la configuración como autoincremento, nos encontraríamos muy rápidamente con que al crear registros en diferentes máquinas de forma descentralizada y luego sincronizarlos, hay colisiones de ID y tenemos que renumerar los registros de forma complicada. Además, si estamos resolviendo muchas sesiones a otras tablas, esto puede ser una sobrecarga muy compleja en la que es fácil cometer errores.

Por lo tanto, en lugar de un identificador numérico, podemos generar un `UUID`, que es una cadena de texto que se genera mediante un complejo algoritmo que garantiza que será único aunque se genere de forma independiente en varias máquinas.

Ventajas:

- Si tienes varias bases de datos independientes que luego sincronizas, el uso de un UUID significa que un ID es único en todas las bases de datos, no sólo en la que estás y en la que se generó. Cuando se fusionan en un solo clúster, no surgen conflictos.
- Puede conocer su "clave principal" antes de insertar el registro en la base de datos. Esto reduce el número de consultas SQL, simplifica la lógica de las transacciones y puede utilizarla fácilmente como clave externa antes de que exista la colección de registros.
- El UUID no revela información sobre el número de fechas y secuencias y es más seguro para su uso en URLs. Por ejemplo, si encuentro que soy el usuario `19010018`, es fácil adivinar que el usuario `19010017` y otros también existen. El ataque se denomina ataque vectorial.

Generar un nuevo UUID
----------------------

El UUID se puede obtener mediante una simple consulta SQL `SELECT UUID();`, pero esto aumenta el número de consultas a la base de datos y perdemos la posibilidad de preparar los datos primero en bloque en la lógica de la aplicación y luego escribirlos de una vez.

Por ello, me gusta utilizar el paquete <a href="https://github.com/ramsey/uuid">ramsey/uuid</a> obtenido por Composer como una buena solución. El UUID en sí mismo tiene varias versiones, y el paquete puede generar juguetonamente todos los tipos según sea necesario.

Esto facilita su uso:

```php
require 'vendor/autoload.php';

use Ramsey\Uuid\Uuid;

// Genera el objeto UUID de la versión 1 (basado en el tiempo)
$uuid1 = Uuid::uuid1();
echo $uuid1->toString() . "\n"; // e4eaaaf2-d142-11e1-b3e4-080027620cdd

// Genera la versión 3 (basada en el nombre y con hash como MD5) del objeto UUID
$uuid3 = Uuid::uuid3(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid3->toString() . "\n"; // 11a38b9a-b3da-360f-9353-a5a725514269

// Genera la versión 4 (aleatoria) del objeto UUID
$uuid4 = Uuid::uuid4();
echo $uuid4->toString() . "\n"; // 25769c6c-d34d-4bfe-ba98-e0ee856f3e7a

// Genera un objeto UUID de la versión 5 (basado en el nombre y con hash como SHA1)
$uuid5 = Uuid::uuid5(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid5->toString() . "\n"; // c4a760a8-dbcf-5254-a0d9-6a4474bd1b62
```

Si utiliza Doctrine, existe una extensión <a href="https://github.com/ramsey/uuid-doctrine">ramsey/uuid-doctrine</a> que genera el ID directamente como un tipo de dato.

Almacenamiento físico en la base de datos
---------------------------

En mis primeros intentos utilicé `varchar(36)` como clave primaria (ID), pero <a href="https://www.facebook.com/groups/backendisti/permalink/2465260887049808/">eso no es para nada una buena idea</a>.

> **Explicación de la lógica interna:**
>
> > Las bases de datos MySql (y muchas otras) no pueden utilizar `varchar`, `char` u otros tipos de datos que expresen una cadena como clave primaria de forma eficiente.
> En algunas bases de datos, existe un tipo de datos `GUID` que está diseñado para almacenar UUIDs directamente. Si no puede utilizar este tipo, existe un sustituto adecuado con la forma `binario(16)`.

Al examinar físicamente la base de datos, el ID se representa en formato HEX (ya que el formato binario no puede mostrarse), en lugar del bonito ID `726c67c4-e5eb-4a4c-8fcc-031da5d6f3c6`, sólo verá `726C67C4E5EB4A4C8FCC031DA5D6F3C6`, que se parece a `'?kYߟKg2c;'` en la consulta INSERT.

Convertir los datos originales de `varchar(36)` a `binario(16)`
----------------------------------------------------

Supongo que representas (o planeas representar) el nuevo ID establecido en la base de datos como:

```sql
`id` binary(16) NOT NULL
```

Sin embargo, el simple hecho de cambiar el tipo de datos no funciona, así que algo como

```sql
SET FOREIGN_KEY_CHECKS=0;

ALTER TABLE article CHANGE id id BINARY(16) NOT NULL

SET FOREIGN_KEY_CHECKS=1;
```

Hay básicamente dos razones:

- La clave primaria y la sesión a ella `deben tener el mismo tipo de datos`. Por lo tanto, es necesario cambiar tanto el tipo de datos para el ID del artículo como, por ejemplo, en la tabla relacional que relaciona los artículos con los autores.
- El formato binario contiene algo ligeramente diferente a la cadena original. Es necesario utilizar una función de conversión.

Por lo tanto, la única solución correcta es hacer una copia de seguridad de los datos (pero, de todas formas, debería hacerlo antes de cada migración), preparar una base de datos vacía con relaciones funcionales y volver a poner los datos allí mediante la migración.

Si ha generado UUIDs de forma extraña antes, es mejor elegir algún método secuencial para obtener el UUID y renumerar todos los registros. La razón es que la disposición secuencial permite ordenar mejor los valores y crear un `btree`, lo que hace que el rendimiento sea casi idéntico al de `bigint`.

**Si conoce una forma mejor de convertir una base de datos existente de UUID almacenado como varchar a formato binario sin tener que idear migraciones complejas y con preservación de las claves foráneas, estaría muy agradecido por los comentarios**.
