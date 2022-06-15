Algoritmo del motor de búsqueda de Internet - Apenas un rastreador
==================================================================

> id: '5ad042bf-7fda-4b2e-a38c-762169d0b594'
> slug:
> 	cs: vyhledavac-barely-a-crawler
> 	es: algoritmo-del-motor-de-busqueda-de-internet-apenas-un-rastreador
> 
> publicationDate: '2016-09-11 12:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '5daf3854688c39f9e0071a93ab11dd4e'

En la lección de hoy, hablaremos de los barriles de datos, de su estructura, de los StopSlovas y, finalmente, describiremos los crawlers.

Barriles de datos
-------------

Se trata de un tipo de datos especial que reside en varios servidores simultáneamente en varias copias. Suelen ser archivos de cientos de GB con gran cantidad de datos y son lentos de leer (por eso se dividen en partes) y prácticamente imposibles de editar. Si queremos hacer un mínimo cambio, tenemos que volver a calcular todo el barril. Por ejemplo, el motor de búsqueda Seznam puede recalcular los barriles de datos como máximo una vez cada pocos días o semanas, mientras que Google lo hace una vez cada pocas horas (y sólo algunas partes, nunca todo a la vez).

Los barriles contienen palabras y sus apariciones en Internet. Puede decirse que se trata de datos brutos de resultados de búsqueda aún no ordenados (a veces están parcialmente ordenados por importancia relativa) y preparados de antemano. La búsqueda propiamente dicha tiene lugar mucho antes de que el usuario formule la consulta de búsqueda y es aquí donde se preparan todos los resultados. La búsqueda en sí misma sería imposible en tiempo real, por lo que el motor de búsqueda básicamente lee los resultados ya preparados aquí (la búsqueda la realiza el componente indexador, que se tratará en un capítulo aparte).

Los barriles contienen toda la información básica necesaria para construir los resultados de la búsqueda de cada palabra que aparece en Internet, por lo que hay que tener en cuenta los problemas de rendimiento cuando se lee secuencialmente. En la práctica, los barriles suelen almacenarse en lotes en varios servidores y también en la memoria RAM para un acceso más rápido. Por ejemplo, el motor de búsqueda de Google no lee los barriles del disco en absoluto, sino que los mantiene completamente en la memoria RAM y sólo guarda una copia de ellos en el disco en caso de que se pierdan datos de la memoria RAM.

Los grandes motores de búsqueda con recursos suficientes también guardan los llamados "barriles de oro", que contienen las páginas más importantes de Internet y se buscan en caso de una gran carga de búsqueda. Por ejemplo, si varios servidores de búsqueda se caen, los barriles de oro atienden temporalmente las búsquedas. Es posible que el motor de búsqueda no encuentre todos los resultados, pero al menos la búsqueda no se interrumpirá por completo, sino que sólo se limitará temporalmente el número de documentos buscados. También es necesario actualizar los barriles regularmente, ya que el número de páginas en Internet aumenta constantemente. Como los barriles suelen tener un tamaño del orden de los gigabytes, no es posible realizar un mantenimiento incremental, sino que es necesario reconstruirlo todo una vez cada tiempo establecido. Por lo tanto, el motor de búsqueda mantiene un barril auxiliar donde todos los datos están en forma de una gran tabla a partir de la cual se construyen los barriles - por ejemplo, Google construye los barriles aproximadamente cada 12 horas. El gran reto es precisamente que los barriles deben estar al menos parcialmente ordenados y reflejar el estado real de Internet. Así, hasta que el motor de búsqueda no actualice los barriles, el usuario no encontrará nuevas páginas web.

Estructura del barril
----------------

En la práctica, un barril se almacena como un gran archivo de texto que contiene el ID de cada documento en el que aparece la palabra buscada y la posición de la aparición de la palabra buscada actualmente.

Ejemplo de un barril muy pequeño con tres páginas:

```txt
[1:5,8,12,27,30;5:1,3,6;12:4,8,10]
```

Este barril de muestra contiene páginas con los identificadores `1`, `5` y `12`. Los números asociados indican la posición de la palabra en el documento. Un barril de este tipo sólo captura las apariciones de una sola palabra, por lo que cada palabra en Internet tiene su propio barril. Cuando se buscan varias palabras clave, es necesario leer varios barriles (uno diferente para cada palabra) y luego realizar las operaciones según el árbol binario (más en los capítulos anteriores).

La intersección suele hacerse recorriendo uno de los documentos y buscando en el otro. Si los barriles son grandes, es posible que no se lean por completo, por lo que los motores de búsqueda a menudo sólo consiguen leer una parte y luego tienen que devolver el resultado, por lo que tiene sentido ordenar los barriles al menos parcialmente, de manera que las páginas más importantes (que probablemente busque el usuario) estén al principio del barril y todo lo demás esté al final del mismo.

StopLead
---------

Quizá piense que para algunas palabras los barriles podrían ser realmente enormes y, por tanto, difíciles de trabajar. Por ejemplo, la palabra "a" o la palabra "1" se encuentran en más de la mitad de los documentos y, sin embargo, no tienen ningún significado para el buscador (porque rara vez se buscan y requieren más palabras circundantes). Estas palabras se llaman Stopwords.

El problema de las StopWords es que no se pueden buscar todas, por lo que los motores de búsqueda sólo muestran una realidad distorsionada de cómo es Internet para la palabra buscada.

Un ejemplo de búsqueda con StopSlovy es la consulta "Praga 1".

El motor de búsqueda sólo da `3.020.000 resultados` para esta consulta. En realidad, sin embargo, hay muchos más sitios de este tipo. Sin embargo, por razones de rendimiento, no se buscan todas.

Buscar opciones de acceso con StopSlovy:

- No indexarlos en absoluto y no hacer barriles para ellos (los pequeños motores de búsqueda lo hacen)
- Indexarlas, pero sólo almacenar las páginas relevantes en barriles (esto es lo que hacen Google y Seznam, por ejemplo)
- Indexarlos como palabras normales y crear barriles completos (esta solución tiene el problema de que los barriles pueden superar fácilmente el tamaño de un disco duro normal y, por lo tanto, no pueden guardarse y pueden tardar segundos en leerse; por lo tanto, esta solución no se utiliza en la práctica o sólo para motores de búsqueda pequeños y especializados)

En general, no existe un enfoque universalmente correcto y ni siquiera Google lo utiliza a la perfección. Se trata de un problema tan grande que podría llevar toda una vida resolverlo. Sin embargo, para los fines de este documento, esta descripción general es suficiente.

Crawler (robot, también araña)
---------------------------

Un crawler es un programa que rastrea sistemáticamente Internet y hace un seguimiento de qué palabras aparecen en qué páginas y también recoge información auxiliar (también llamada meta información). Un rastreador suele funcionar en muchos servidores, porque el rastreo de Internet es exigente en términos de ancho de banda de la red y, por tanto, hay que rastrear muchas páginas simultáneamente. Google calcula que se rastrean hasta 30.000 páginas simultáneamente en un momento dado.

Antes de abrir una página, el Crawler consulta una base de datos con las direcciones a las que piensa ir en el futuro. Si ya ha estado en la dirección, sólo actualizará sus registros (va a los sitios principales cada pocas horas, a los sitios de noticias cada pocos minutos, a los sitios habituales una vez al mes como máximo).

Si el robot llega a una nueva dirección que no conoce, mirará los enlaces contenidos a otros documentos (páginas). Para cada enlace, calcula su importancia, y si es un enlace importante, también llega a la página más tarde.

Por ejemplo, List sólo rastrea unos pocos niveles de enlaces en cada sitio de esta manera, mientras que Google intenta rastrear todo el sitio, incluidos los documentos sin importancia, lo que lo convierte en el motor de búsqueda más completo de Internet.
El robot también intenta clasificar la página mientras la rastrea y calcula su "rango", que es una representación numérica de la importancia de la página en Internet. En el pasado, Google ha utilizado el PageRank, que tiene un rango de 0 a 10. Google sólo calcula el PageRank 10 para sí mismo y para sitios como microsoft.com o w3c.org. Hoy en día calcula el valor de forma diferente (inteligencia artificial y matemáticas vectoriales).

El PageRank más alto en la República Checa es el de Seznam.cz con un valor de 7. El rango tiene una gran influencia en la frecuencia con la que un robot visitará la página y en la posición que ocupará en los resultados de búsqueda. Los rangos se calculan sólo a partir de los backlinks que apuntan a la página deseada.

El rastreador no hace nada más con los datos, sólo los descarga de Internet y los almacena en una base de datos donde espera el proceso de indexación.
