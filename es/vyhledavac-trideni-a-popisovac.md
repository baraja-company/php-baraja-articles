Algoritmo de los motores de búsqueda en Internet - Clasificación y descriptores
===============================================================================

> id: ca4aaac5-0cd8-41db-b860-c32661c43d82
> slug:
> 	cs: vyhledavac-trideni-a-popisovac
> 	es: algoritmo-de-los-motores-de-busqueda-en-internet-clasificacion-y-descriptores
> 
> publicationDate: '2016-09-11 13:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '541310dfdc96b75a320c7cad1b03e734'

En esta lección sobre los principios de un motor de búsqueda en Internet, comprenderemos cómo un motor de búsqueda clasifica, describe y evalúa los resultados.

Clasificación de los resultados
----------------

Imaginemos un barril terminado que está listo en el servidor de búsqueda. Nuestra primera consulta de búsqueda proviene del usuario y ahora tenemos que hacer la primera ordenación "aproximada", que se refinará posteriormente.

Veamos el siguiente ejemplo de consulta de entrada:

```txt
[O (and) pejskovi (and) a (and) kočičce]
```

Sí, esta es la forma en la que el servidor de búsqueda recibe la consulta procesada del usuario y ahora espera que se devuelva el resultado. En total tenemos menos de `300 ms` para hacer esto, así que rápido :)

En el primer paso, obtenemos barriles llenos de futuros IDs de resultados (también llamados candidatos) y operaciones a realizar. Este barril recuperado puede no estar completo en algunos casos, sino que contiene sólo los valores que el servidor consiguió leer del disco en algún intervalo de tiempo predeterminado (también llamado **timeout**). Por ejemplo, obtenemos las siguientes series de números (que están parcialmente ordenados de antemano):

| Barriles | Documentos |
|-------|---------|
| o | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| perro | 5,19,23,42,46,58 |
| a | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| coño | 9,19,42,57,58,62,68,83 |

Ahora realizamos una intersección gruesa de todos los conjuntos y obtenemos una lista de identificadores de documentos que son iguales en todos los barriles. En la práctica, pasamos por la lista más corta y buscamos en las demás.

Las identificaciones comunes en todos los barriles son "19, 42, 58", por lo que la futura página de resultados contiene 3 elementos en un orden aún desconocido. Seguimos viendo los documentos como identificaciones porque esto ahorra una gran cantidad de datos. Sin embargo, en los resultados de la búsqueda, por lo general no queremos mostrar al usuario los números de los documentos, sino el título del documento (encabezamiento), la descripción, la URL y otra información. Esto lo proporciona el componente "descriptor".

Descriptor
---------

Del capítulo anterior nos queda una lista de candidatos a resultados futuros. Lo recuperamos de forma ordenada en función del sistema, es decir, tal y como el buscador los clasificó secuencialmente en el índice. En este punto, ya no podemos realizar una lectura secuencial del big data, sino que debemos recorrer secuencialmente algún tipo de base de datos relacional para encontrar información adicional para otro tipo de ordenación más comprensible para el usuario.

Por ejemplo, un trozo de base de datos podría tener el siguiente aspecto:


| ID, Título, Etiqueta, URL, Rango, etc.
|----|---------|---------|-----|------|
| 19 | Josef Čapek: Cuento de un perro y un gato | Eso fue cuando el perro y el gato seguían cultivando juntos; tenían su casita junto al bosque y vivían allí juntos y querían hacerlo todo como lo hacen los grandes | www.troglodytarium.cz/webz/axf/.../Capek_J_Pejsek_a_kocicka.htm | 72 |
| 42 | Hablando del perro y del gato | "Está bien", dijo el perro, y el gato tomó jabón y una olla de agua, y se arrodilló en el suelo, y tomó al perro como cepillo, y fregó todo el suelo con el perro. El suelo estaba todo mojado | www.capek.narod.ru/povidani.htm | 86 |
| 58 | Cómo el perro y el gato hicieron una tarta para la fiesta | Mañana era la fiesta del perro y el cumpleaños del gato. Los niños lo sabían y querían sorprender al perro y al gato por su cumpleaños. Estaban pensando qué podían hacer por el perro y | a.da.mek.sweb.cz/capek.j/dort.htm | 34 |

Cálculo de la relevancia
-----------------

El último paso es calcular la relevancia de la respuesta. Para la mayoría de los resultados, basta con representar la relevancia como un único número con un radio fijo por el que se ordenarán los resultados. Por rango, los resultados se clasifican de nuevo "aproximadamente" en varios grupos (el número de grupos depende de la varianza de los rangos) y se sigue trabajando con estos grupos.

Los grupos con mayor rango se toman primero, a menudo esto es suficiente para ordenar la primera página de resultados y no tenemos que ocuparnos de nada más. Sin embargo, si varios documentos diferentes tienen el mismo rango, tenemos que hacer un análisis más detallado y calcular rangos auxiliares adicionales para ayudar a clasificar la página. El rango adicional puede ser, por ejemplo, la calidad de los enlaces, la antigüedad del documento, la longitud del título, el "look and feel" de la URL y muchos otros factores.

Devolución de resultados al usuario
---------------------------

¡Hurra! Ya tenemos los resultados y ahora sólo queda renderizar la página para el usuario. El paquete de resultados se devuelve al servidor, que encaja los resultados en una plantilla preparada de antemano, inserta banners publicitarios en la página y la devuelve al usuario. Al mismo tiempo, también puede realizar otras operaciones auxiliares, como el almacenamiento en caché de los resultados. Si otra persona busca la misma consulta en algún momento en el futuro cercano (por ejemplo, en la última hora), los resultados no se buscarán de nuevo, sino que sólo se leerán de la memoria temporal, lo que suele ayudar a mejorar el rendimiento general del motor de búsqueda.

Conclusión y visión de la semántica
---------------------------

Este artículo pretendía describir los principios básicos del funcionamiento interno de los motores de búsqueda y cómo consiguen recuperar un número teóricamente ilimitado de documentos en un tiempo todavía razonable. Se trata de enormes optimizaciones matemáticas que han sido desarrolladas durante muchos años por los mejores equipos de programación.

Una descripción completa de los detalles está fuera del alcance de este documento. Todo esto se complica también por el hecho de que la mayoría de los otros pasos no están descritos en detalle por ninguno de los motores de búsqueda, porque son sus secretos comerciales.

También es importante tener en cuenta que los motores de búsqueda todavía no entienden el contenido de las páginas ni el significado de los resultados, y que sigue siendo sólo un cálculo estadístico basado en la potencia de cálculo bruta y en la capacidad de la gente para enlazar con textos de calidad, y este sistema también es bastante vulnerable (tomemos el ejemplo de la "bomba de Google", en la que un webmaster furtivo fuerza un contenido en una consulta de búsqueda que es irrelevante).

Este problema debería ser resuelto en parte por la inteligencia artificial, que todos los motores de búsqueda están tratando de integrar gradualmente. Sin embargo, se trata de una música aún lejana del futuro, ya que no es una tarea fácil y en su mayor parte se trata de mejorar los métodos de cálculo estadístico. Pongamos como ejemplo la búsqueda gráfica de Google, que trata de responder a la consulta directamente (aunque en su estructura interna se limita a buscar en una base de conocimientos predefinida).

Así que la próxima vez que haga una consulta a su motor de búsqueda favorito, recuerde cuánto trabajo tuvo que hacer su motor de búsqueda y cuántos TB de datos tuvo que leer del disco para encontrar los 10 mejores documentos para su consulta de búsqueda.
