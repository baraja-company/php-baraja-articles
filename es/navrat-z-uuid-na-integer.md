Retorno de UUID a entero
========================

> id: d842517b-c4f6-4cef-a839-d08ff3804fca
> slug:
> 	cs: navrat-z-uuid-na-integer
> 	es: retorno-de-uuid-a-entero
> 
> publicationDate: '2021-05-02 17:00:00'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: b7c136f3e3ca8a4563230c557ab1fa9c

En el desarrollo de software, un programador se encuentra muy a menudo en un callejón sin salida cuando se enfrenta a una decisión de arquitectura que tendrá un gran impacto en el futuro de su trabajo durante décadas. Al mismo tiempo, es una decisión que no tiene marcha atrás, y cada error tiene un precio muy alto. Las bases de datos son un ejemplo típico de decisión arquitectónica en la que ruedan cabezas con cada pequeño error.

Una de las grandes decisiones de los últimos tiempos ha sido cómo almacenar las claves primarias en las tablas de las bases de datos. Aunque parece un problema trivial, hay mucho más detrás de él de lo que se cree.

Opciones de clave primaria
-------------------------

Hay básicamente cuatro opciones básicas:

- entero
- entero sin signo
- gran int
- <a href="/uuid-performance">UUID</a>

Integer es simplemente un entero (en el caso de `unsigned` entonces sin signo, por lo que siempre es positivo, y en el caso de `big int` entonces puede alcanzar valores extremadamente grandes). Un concepto muy simple. Un UUID es entonces una cadena de texto (por ejemplo, de la forma `c4a760a8-dbcf-5254-a0d9-6a4474bd1b62`) que consta de varias partes, cada una de las cuales puede tener ciertas propiedades y son útiles para construir enormes aplicaciones multiservidoras o descentralizadas. existe un gran ecosistema de tecnologías útiles en torno a los UUID que resuelven problemas que quizá ni siquiera sepas que tienes, o que tendrás en el futuro.

Utilizar el martillo adecuado
-------------------------

No hace mucho tiempo (invierno de 2020) mi amigo Paul explicaba el concepto de aplicar la solución adecuada a un problema de un tamaño determinado. Esta es una gran e importante idea que a muchos desarrolladores les gusta olvidar: crea soluciones enormemente complejas cuando no es necesario. En inglés, tenemos una bonita frase para esta **sobreingeniería**.

Tamaño y unicidad del UUID
--------------------------

La ventaja fundamental del UUID es que si la aplicación se hace demasiado grande y dividimos la base de datos entre muchos servidores web, donde una tabla de la base de datos es tan enorme que no cabe en el disco de una sola máquina, podemos dividirla entre muchas máquinas físicas, cada una de las cuales conocerá su propia parte de la tabla y consultará a sus colegas por el resto. UUID también resuelve el problema fundamental de la inserción de nuevas filas, cuando en el caso de aplicaciones extremadamente grandes necesitamos escribir filas en paralelo en muchos lugares y no queremos esperar a que se libere la capacidad de escritura del servidor principal.

El concepto de escribir en muchos lugares al mismo tiempo es utilizado, por ejemplo, por las aplicaciones de chat. Cuando envías un mensaje a través de Messenger, éste va al servidor de la base de datos de Facebook más cercano, que asigna un UUID y una marca de tiempo al mensaje y lo escribe en su base de datos local. Su amigo del otro lado del mundo, a su vez, escribe los mensajes en su centro de datos local y, mientras tanto, toda la infraestructura de la nube garantiza la sincronización en todo el mundo. Suena bien, ¿verdad? :)

Para que esta escritura paralela funcione, es necesario resolver el problema de la colisión de registros. Si las bases de datos locales individuales utilizan un simple número entero, muy pronto dos servidores independientes escribirán dos registros diferentes con el mismo identificador. Al sincronizar estos registros, se producirá una colisión. Por lo general, no hay solución: no se pueden renumerar las identificaciones, porque otras sesiones pueden llevar a eso.

UUID resuelve esto, por ejemplo, dando a cada servidor un prefijo acordado que inserta al principio de cada UUID, luego inserta una marca de tiempo y luego el identificador mismo.

> **Hecho interesante:** Al escribir una cantidad tan grande de datos, no nos interesa tanto cuándo se escribió qué registro, sino en qué orden se escribió (para no cambiar el orden de los mensajes para el usuario, por ejemplo).

Puede que te preguntes cómo manejar una situación en la que los servidores no se ponen de acuerdo entre sí sobre qué prefijo utilizar. Este problema se plantea, por ejemplo, en las aplicaciones descentralizadas u offline. En este caso, el UUID puede incluso generarse aleatoriamente.

La pregunta entonces es qué posibilidades hay de que se produzca un conflicto al <a href="https://stackoverflow.com/questions/1155008/how-unique-is-uuid">generar aleatoriamente un UUID</a>. Bueno, probablemente no te pase a ti. Hay aproximadamente `2^122` UUIDs únicos (porque es un `número de 128 bits`). En la práctica, la probabilidad de que se produzca un conflicto es de aproximadamente `0,00000000006 (6 × 10-11)`. En la práctica, esto significa que si generamos **1.000 millones de UUID** cada segundo durante los próximos 100 años, la probabilidad de conflicto será del `50%`. Por lo tanto, es más probable que no se produzcan conflictos, y el UUID es la solución definitiva a sus problemas de base de datos.

¿Es necesaria una solución tan sólida?
-------------------------------

Si no lo sabes, la respuesta es **no**.

Con la clave primaria establecida como `int` con la bandera `unsigned`, hay `4.294.967.295` valores posibles (4 mil millones). Para comparar los tamaños de los enteros, consulte la <a href="https://dev.mysql.com/doc/refman/8.0/en/integer-types.html">documentación de MySql</a>.

Si almacena 4.000 millones de registros en una tabla, probablemente se quedará antes sin espacio en el disco.

Rendimiento de los enteros y las uniones
----------------------

Los números enteros son realmente muy rápidos. Hay optimizaciones nativas para ellos en MySql. Los índices funcionan correctamente (además son mucho más pequeños), sólo ocupan 4 bytes, son muy rápidos de unir, y estarán bien para la mayoría de los casos.

Si se trata de un problema de replicación de la base de datos, la mejor solución podría ser poner toda la base de datos en una nube como MS Azure y consultarla externamente. Incluso cuando se almacenan decenas de millones de registros, la velocidad de acceso a través de enteros a una fila concreta es del orden de milisegundos (menos de 3 ms en un servidor bien configurado), y con un índice agrupado, el tiempo puede ser bien sostenible incluso con un gran número de peticiones.

Si realmente necesitas usar UUIDs, es mejor que dejes el mundo de MySql y vayas por el camino de la base de datos Postgres, que a diferencia de MySql tiene su propio tipo de datos para <a href="https://www.postgresql.org/docs/9.1/datatype-uuid.html">UUUIDs</a>. Trabajar con uniones es un gran problema con UUIDs y MySql, y cuando se unen sólo 3 tablas (con cada una teniendo sólo sub decenas de miles de registros), la consulta completa puede tomar varios cientos de milisegundos a bajos segundos para procesar. Y esto es desgraciadamente un problema de MySql que probablemente no puedas resolver.
