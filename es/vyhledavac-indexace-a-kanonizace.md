Algoritmo de los motores de búsqueda en Internet - Indexación y canonización
============================================================================

> id: daa97e66-e77f-4741-ade2-905c1cfdbd56
> slug:
> 	cs: vyhledavac-indexace-a-kanonizace
> 	es: algoritmo-de-los-motores-de-busqueda-en-internet-indexacion-y-canonizacion
> 
> publicationDate: '2016-09-11 11:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d73a30f413e4b8b77dbceda60b3cf173

En la lección de hoy, veremos la indexación y canonización de documentos en Internet.

Indexación
--------

El proceso de indexación lo realiza un componente llamado indexador. Se trata de un programa especialmente diseñado que convierte los datos descargados (los datos que el Crawler ha descargado) en un tipo de datos especial para la búsqueda - barriles.

El problema de la indexación es que no se puede navegar "inteligentemente" por los documentos, pero la lectura secuencial (leer todo el texto de principio a fin) es inevitable, por lo que es una disciplina exigente y los motores de búsqueda utilizan los servidores más potentes para esta actividad. Ninguna otra tarea del proceso de búsqueda es tan exigente como la indexación, donde el texto plano se convierte en índices.

Por ejemplo, una página sobre gatos, descargada de Wikipedia. El indexador obtiene el texto completo de la página y tiene que eliminar las cosas innecesarias (por ejemplo, los menús de control del usuario, los anuncios, los pies de página, ...) y analizar la página para obtener el texto plano. Por ejemplo, el texto podría ser:

> El gato doméstico (Felis silvestris f. catus) es una forma domesticada del gato salvaje que ha sido compañero de los humanos durante miles de años. Al igual que su pariente salvaje, pertenece a la subfamilia de los gatos pequeños y es un representante típico del grupo. Tiene un cuerpo ágil y musculoso, perfectamente adaptado a la caza, garras y dientes afilados, y una excelente vista, oído y olfato.

Extraído de [Wikipedia](http://cs.wikipedia.org/wiki/Ko%C4%8Dka_dom%C3%A1c%C3%AD).

El motor de búsqueda ahora analiza las palabras individuales del texto y las escribe en barriles individuales. Sin embargo, no puede hacer la escritura del barril directamente (principalmente porque cada barril existe en muchas copias y también porque interferiría con la búsqueda), así que crea una lista de requisitos de cómo debe ser el futuro barril y los almacena en la memoria temporal. En nuestro caso, dicha lista podría ser algo así (algunas palabras sin importancia pueden ser ignoradas o marcadas como palabras de parada):

| ID del documento, palabra, posición, tipo, etc.
|--------------|-------|--------|-----------|
| 1 | Gato | 1 | Normal |
| 1. Doméstico. 2. Normal.
| 1 | Felis | 3 | Normal |
| 1. Es 7. Palabra de parada.
| 1. Domesticado. 8. Normal.

Esta lista es enorme (mucho más grande que los textos originales), pero sigue siendo más rápida de buscar porque no tenemos que leer todos los textos secuencialmente, sino que podemos buscar por índice en cada columna y las palabras de una página se ordenan en filas una tras otra.

Después de que haya pasado algún tiempo (o se haya completado algún número de documentos), el indexador deja de trabajar en la construcción de esta lista de peticiones (para un futuro barril) y comienza a leer los datos de nuevo y a reconstruir los barriles individuales (esta lista incluye los registros antiguos que están en un barril que ya funciona). Si se añaden nuevos registros de direcciones conocidas, este proceso los actualizará, mientras que para los nuevos documentos los incluirá.

De este modo, el indexador volverá a recorrer la lista y creará poco a poco nuevos barriles que contengan todos los elementos (tendrán el mismo aspecto que el mostrado en el ejemplo de los capítulos anteriores) y cuando se completen todos los barriles, se enviarán a los servidores de búsqueda individuales. La actualización de los barriles en los servidores lleva mucho tiempo (principalmente debido a la gran cantidad de datos que se mueven), por lo que durante esta operación los servidores no están disponibles y los datos se actualizan en diferentes servidores en diferentes momentos. Esto provoca, por ejemplo, que algunos usuarios puedan obtener resultados diferentes porque cada uno de ellos busca en un servidor de dispensación diferente (debido a la descomposición de la carga). Una vez completada la actualización, todo vuelve a la normalidad y todos los usuarios encuentran todos los documentos por igual.

El proceso de indexación es importante para todos los motores de búsqueda, y el que lo hace más a menudo y con más cuidado es el que tiene la visión más actualizada de Internet. Google realiza esta operación cada pocas horas, Seznam una vez a la semana (y tiene un millón de veces menos datos).

Canonización de documentos
--------------------

En el diseño original del motor de búsqueda de texto completo, no había necesidad de nada como la canonización porque Internet era un medio que creaba constantemente nuevos contenidos. Sin embargo, con el tiempo se ha producido una duplicación (es decir, que el mismo contenido aparezca en varias URL diferentes) y los motores de búsqueda deben adaptarse a ello. Un ejemplo típico es Wikipedia, que tiene muchos artículos. Algunos autores de otras páginas se apropian de estos textos (en parte o incluso en su totalidad) y crean así una duplicación. En la mayoría de los casos, sin embargo, esto no importa, porque la página de origen tiene un rango (calidad de los enlaces) mucho más alto que la plagiada, pero a veces puede ocurrir que degrade la original en detrimento de la pirata.

Los motores de búsqueda han tenido que adaptarse a este vicio y se ha acuñado el término "canonización", que puede entenderse como la "eliminación" de ciertas páginas del índice. Por cierto, esto hace que los índices sean más pequeños y que el motor de búsqueda no tenga que rastrear el mismo contenido todo el tiempo innecesariamente.

Los duplicados se dividen internamente en 2 grandes categorías por cada motor de búsqueda:

Duplicados naturales
-------------------

Se crean por el comportamiento natural de Internet y por sus características.

Por ejemplo, es probable que la URL `http://mathematicator.cz` tenga el mismo contenido que la URL `http://www.mathematicator.cz` o `http://mathematicator.cz/index.php` porque este es el comportamiento estándar del servidor Apache (e Internet en general).

Si se encuentra una duplicación natural, el motor de búsqueda crea un "conjunto canónico", que es un grupo de páginas del que el motor de búsqueda selecciona un representante que se destaca en la búsqueda. Si un enlace lleva a cualquier página del conjunto canónico, su rango se pasará automáticamente al representante principal.

A menudo es una buena idea ayudar al motor de búsqueda a crear este conjunto y configurar la redirección correctamente en el sitio, lo que hará que el motor de búsqueda mire mejor el sitio y el representante principal sea seleccionado mejor.

Duplicados que conducen al plagio
----------------------------

El plagio es un problema en Internet hoy en día. La existencia del plagio en sí no tendría mucha importancia, sin embargo, a los usuarios les molesta especialmente porque siguen encontrando los mismos resultados para la misma consulta. ¿Ha encontrado alguna vez varias páginas con un texto idéntico para una consulta? Este es exactamente el comportamiento que los motores de búsqueda intentan evitar.

El mayor problema es determinar qué página es la fuente original, y hacerlo a máquina. De nuevo, los motores de búsqueda ponen todas las páginas similares en un conjunto canónico y seleccionan el representante principal de ese conjunto. Si las fuentes son de diferentes dominios, la situación no puede considerarse como una duplicación natural (y elegir cualquier candidato), sino que hay que evaluar cualitativamente todas las páginas y seleccionar objetivamente la mejor, e idealmente la fuente original del contenido.

Los motores de búsqueda suelen decidir basándose en el rango de todo el dominio y en la fuerza de la red de enlaces a un determinado documento, pero incluso esto es un enfoque poco fiable. El segundo factor suele ser también el momento de creación (indexación) del documento. Por lo tanto, cada motor de búsqueda hace un seguimiento de las páginas que suelen generar nuevos contenidos y las visita con frecuencia, de modo que, en el mejor de los casos, se da cuenta de la nueva página inmediatamente y, por lo tanto, no elige otra página como proxy.

Una descripción detallada de los métodos de funcionamiento de esta selección va más allá del alcance de este artículo y podría ser objeto de un libro entero.
