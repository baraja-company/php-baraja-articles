Algoritmo del motor de búsqueda de Internet - Trees y StopLead
==============================================================

> id: '11ec73f2-e505-4338-89aa-8307a55b7ba0'
> slug:
> 	cs: vyhledavac-stromy-a-stopslova
> 	es: algoritmo-del-motor-de-busqueda-de-internet-trees-y-stoplead
> 
> publicationDate: '2016-09-11 10:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d011d2d6943271ca5e45c3d3cdf03d61

Cada segundo se añaden 5 millones de páginas nuevas a Internet, y este ritmo no deja de aumentar. Para dar un poco de orden a este inmenso mar de información y encontrar algo en él, existen los motores de búsqueda. El siguiente trabajo pretende introducir el tema de la búsqueda y explicar todo el proceso desde la creación de una nueva página hasta su localización en un motor de búsqueda.

La tarea de encontrar y clasificar un conjunto de miles de millones de documentos no es fácil. Sólo Google necesita 300.000 servidores web para realizar esta tarea en pocas horas. De hecho, la búsqueda de su consulta tiene lugar mucho antes de que usted la plantee. Google ya tiene almacenados en su memoria los resultados de las búsquedas que vas a realizar en los próximos días.

Arquitectura del motor de búsqueda
------------------------

<img src="{$baseUrl}/images/fulltext-schema.png" alt="Arquitectura de motor de búsqueda común para hasta 50 millones de documentos" class="w-100 mb-3">

Un motor de búsqueda bien diseñado contiene muchos componentes, que son del orden de varios cientos. Este diagrama describe los elementos más básicos que un motor de búsqueda debe contener como mínimo para funcionar. Se trata de una arquitectura antigua, que ya no se utiliza, y que funcionó hasta aproximadamente 2005, cuando sólo había unos pocos millones de documentos en la web. Esta arquitectura no permite una cantidad "infinita" de contenidos y también la posible caída de los servidores de búsqueda (búsqueda base). En el caso de que un solo componente se caiga, todo el sistema dejará de funcionar y el motor de búsqueda no estará disponible.

Una descripción completa de las tendencias actuales en el diseño de nuevas arquitecturas queda fuera del alcance de este artículo, ya que suelen ser secretos comerciales de las grandes empresas. El objetivo de este artículo es explicar cómo funciona esta arquitectura básica. Los componentes individuales se explican en detalle en los siguientes capítulos.

Entrada de la consulta
------------

El componente más visible, incluso para los usuarios ordinarios, es la entrada de la consulta, que actualmente se representa con mayor frecuencia como un cuadro de texto. El campo de entrada se encuentra en una página especial que suele ser sólo para la entrada.

En la siguiente etapa, el usuario introduce la cadena de búsqueda y envía el formulario a los servidores del motor de búsqueda, iniciando así la comunicación con el componente de dispensación, a veces también llamado búsqueda principal. Los servidores de distribución están construidos para manejar grandes cargas de usuarios y deben ser capaces de manejar todos los buscadores a la vez.

La arquitectura de un servidor de despacho suele ser un simple servidor Apache con una instalación básica y un excelente ancho de banda de red. Cuando una consulta entra en el servidor, se procesa a través de árboles de decisión de regresión y se crea un "mapa de consulta" que captura la semántica completa en torno a los otros significados para dejar claro lo que el usuario está buscando realmente. Los métodos de reescritura de consultas están demasiado lejos del alcance de este artículo, por lo que nos limitaremos a mostrar descripciones generales de lo que puede y no puede ser la reescritura.

Considere la siguiente consulta:

```txt
"O pejskovi a kočičce"
```

Esta consulta se convertirá en un árbol binario que captura su esencia semántica:

<img src="{$baseUrl}/images/fulltext-tree.png" alt="Diagrama de árbol de parseo simbólico" class="w-100 mb-3">

El objetivo de un árbol de análisis sintáctico es capturar la forma en que un motor de búsqueda mirará los resultados de la búsqueda. Este árbol, junto con la consulta, se envía a los servidores de búsqueda auxiliares (etiquetados como Búsqueda de base en este documento), donde se buscan las palabras clave individuales (lecturas de índice) y luego se ordenan según las reglas (por operaciones).

| Operador | Operaciones de clasificación |
|----------|------------------
| Y | Intersección |
| OR | Suma |
| NOT | Complemento |

No hay una clasificación real de los resultados de la búsqueda, este componente sólo realiza operaciones binarias rápidas en grandes cantidades de datos (a menudo millones de registros) y básicamente sólo compara los ID de los documentos individuales.

El funcionamiento de la clasificación de los resultados de búsqueda en la página de resultados se discutirá en las siguientes secciones. El servidor de salida simplemente se utiliza para combinar una gran cantidad de datos de diferentes servidores de búsqueda para aumentar el rendimiento general y repartir la carga, y luego introduce los valores detectados en la página de resultados (esto se llama "página web"), que contiene información compuesta de docenas de servidores.

Reescritura en un árbol binario
-----------------------

Como se mencionó en el capítulo anterior, toda la búsqueda se basa en árboles binarios que le indican cómo serán los resultados y qué debe buscar. Toda la "lógica" e "inteligencia" de un motor de búsqueda depende directamente de la calidad del programa que crea el árbol, es decir, el descriptor.

Un árbol correctamente construido debe contener toda la metainformación necesaria sobre la consulta de tal forma que pueda procesarse fácilmente incluso con la ayuda de la lectura secuencial del disco y debe eliminar los accesos aleatorios a la memoria, por lo que suele contener las palabras individuales de la búsqueda (del usuario), sus transcripciones (y formas deletreadas) y, por último, pero no menos importante, los enlaces entre las palabras, es decir, sus distancias relativas en el texto.

Métodos de transcripción de consultas
---------------------

La forma más básica de transcribir una consulta es analizarla en palabras individuales (según los espacios), con las que se trabajará posteriormente. El segundo paso es buscar las StopWords y sustituirlas por un puntero variable (porque, como mostraremos más adelante, no tiene sentido buscar las StopWords en absoluto).

Tengamos la siguiente consulta: "Sobre un perro y un gato", su transcripción en su estado más básico tiene el siguiente aspecto:

```txt
"O (and) pejskovi (and) a (and) kočičce"
```

El segundo paso es eliminar las palabras que no tienen relevancia para la búsqueda. Por supuesto, por ejemplo, la palabra "sobre" o "y" tiene sentido para nosotros los humanos, pero no para una búsqueda en una base de datos, porque puede ser sustituida por otra palabra y los resultados listados. El objetivo no debe ser encontrar una coincidencia literal de la consulta con el índice, porque eso llevaría a resultados erróneos, ya que a menudo las palabras en el texto en Internet están en diferentes grafías, y este método intenta encontrar resultados en otras formas que las que obtuvimos del usuario. Se trata, pues, del primer indicio de un motor de búsqueda "inteligente".

Estas palabras sin sentido suelen llamarse "stopwords", cada motor de búsqueda debería mantener un índice de dichas palabras y este índice debería actualizarse con frecuencia (manualmente).

```txt
["pejskovi (and) * (and) kočičce"]
```

En este punto, buscamos 2 palabras diferentes ("perro" y "gato") con una palabra adicional en medio (internamente, la marcamos con un asterisco).

El objetivo de esta modificación no es aumentar la calidad de la búsqueda, sino el rendimiento. Esto se debe a que cuando buscamos una palabra demasiado frecuente, se produce una carga rápida, y esta modificación ayuda al proceso, especialmente al no buscar una palabra que estará disponible en esa posición en la mayoría de los documentos de todos modos.

También es importante tener en cuenta que no siempre podemos hacer este ajuste (omitir una palabra), por lo que cada motor de búsqueda debería mantener un índice separado de las frases que se encuentran en Internet para comprobar qué ajuste da buenos resultados y cuál no. Sin embargo, ocasionalmente un motor de búsqueda hará este ajuste aunque no tenga la frase en su índice - especialmente cuando un usuario está buscando una palabra importante que se espera que tenga páginas importantes en las primeras posiciones, lo que supera este "error" porque los usuarios suelen solicitarlas de todos modos.

El último paso es trabajar con el idioma inglés y "limpiar" la consulta de caracteres innecesarios. Por ejemplo, en la consulta "lavadora", el usuario suele esperar resultados sólo con la palabra "lavadora" y podemos ignorar el carácter de la coma.

No se conocen los métodos exactos de funcionamiento de este sistema y cada motor de búsqueda tiene el suyo propio. Se cree que, en su mayor parte, se trata de un modelo estadístico que realiza estos ajustes a partir del conocimiento de miles de millones de textos en la base de datos.

La transcripción del inglés también es una ciencia en sí misma, por lo que aquí también se dará sólo una descripción básica. En la mayoría de los casos, basta con añadir a cada palabra sus formas con guiones (según el diccionario) y buscarlas junto con la palabra, consiguiendo así que el buscador pueda encontrar documentos en los que las palabras no aparezcan sólo en su forma básica (tal y como las introdujo el usuario), sino que también pueda encontrar las versiones flexionadas, una característica muy útil. El problema de este enfoque está en la inflexión de palabras oscuras y problemáticas, y también en la inflexión de consultas que son cortas (y por lo tanto no es posible determinar por el contexto cómo inflexionar la palabra). La palabra "juegos" es un ejemplo de inflexión problemática.

Dada una consulta en inglés, "I love her", un inflexionador mal diseñado inflexionará la palabra "games" como, por ejemplo, "games, gaming, gaming room, ...", por lo que también es necesario indexar las palabras circundantes como frases y realizar la inflexión sólo en situaciones familiares, o utilizar un procedimiento diferente (aquí es donde suelen entrar los algoritmos fonéticos).

El último paso consiste en enviar el árbol terminado a los servidores de búsqueda de la Base, donde tendrá lugar la búsqueda real del barril - este es el tema del próximo capítulo.
