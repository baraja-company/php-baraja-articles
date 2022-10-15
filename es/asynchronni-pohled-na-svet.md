Visión asíncrona del mundo
==========================

> id: '7ed25269-659f-4d17-a642-d07480cbfafa'
> slug:
> 	- null
> 	es: vision-asincrona-del-mundo
> 
> cs: asynchronni-pohled-na-svet
> publicationDate: '2022-10-15 11:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '24d4ec106e7febec5ee84cdf86bd8f28'

Hace tiempo que me di cuenta de que nuestro mundo tiene una naturaleza asíncrona y descentralizada. Cuando te das cuenta de esto, y empiezas a pensar en cómo utilizarlo en tu beneficio, puede surgir fácilmente todo un gran concepto de cómo ver la resolución de problemas complejos. En este post, me gustaría explicar algunas de las ideas que ya estoy utilizando. No las obtengo de ninguna fuente en particular, es más bien una combinación de múltiples experiencias y mis propias ideas. Estos principios no sirven para todos los casos.

Definir el entorno y los objetivos
-------------------------

Casi todos los proyectos que he abordado (solo o en equipo) han tenido un objetivo bastante definido. Este enfoque tiene mucho sentido porque es importante saber lo que queremos. Por otra parte, creo que definir un objetivo concreto es imposible en un entorno complejo, y a menudo durante la ejecución nos encontramos con que en realidad queremos llegar a otro sitio.

Así que en lugar de un objetivo específico, construyo un área de objetivos diferentes donde hay alternativas, o incluso un objetivo independiente aislado completamente nuevo puede ser el objetivo correcto si tiene sentido.

Ejemplos de la práctica:

- Tengo que expandirme gradualmente a otros mercados. Uno de los subobjetivos que descubrí durante la implementación puede ser la traducción automática de la web.
- Tengo que recoger 6 envíos en diferentes lugares de Praga y no sé cuál es la ruta más corta. No quiero descubrirlo de forma complicada, así que simplemente me dirijo hacia el punto más lejano y cambio mi plan hacia otras rutas en el camino. Por ejemplo, cuando cambio en Anděl, decido tomar el tranvía que llega primero, lo que afecta dinámicamente al siguiente plan de ruta.
- Necesito escribir este post, pero no tengo una hora de tiempo seguida. Por lo tanto, puedo escribirlo por partes mientras voy en el metro como capítulos individuales. Luego haré la fusión final en este formulario en el futuro.
- No sé cómo funciona el algoritmo para calcular el canje de la mercancía para mi cliente, que tengo que reprogramar. Así que plantearé la consulta a varias personas al mismo tiempo y resolveré otra cosa mientras lo hago. De forma asíncrona durante 2 días, llegarán múltiples respuestas diferentes, sobre las que sólo tomaré una decisión en base a ellas más tarde.
- Necesito deshacerme de PHP en el servidor durante mucho tiempo, así que estoy reescribiendo gradualmente una página a la vez en React. Poco a poco estoy trasladando los sitios a la nube y construyéndolos en Nect generados estáticamente.

Ninguno de los destinos es definitivo. Si trabajas con una superficie y no con un punto, es mucho más probable que aciertes. Al mismo tiempo, la motivación funciona bien en este caso, porque lo peor que puede pasar es alcanzar tu objetivo y luego no tener algo que esperar en el futuro.

También es importante decir que para tener una mentalidad similar es necesario tener un entorno bastante estable en el que se puedan cometer errores. Este enfoque no puede garantizar un plazo específico para resolver una sola tarea concreta. Por otro lado, puede optimizar la resolución del mayor número de tareas paralelas en el menor tiempo posible, aunque no necesariamente en el orden en que llegaron a la cola.

El principio podría compararse con el funcionamiento de los cálculos en la programación paralela, por ejemplo. En resumen, se descomponen las tareas en hilos individuales que resuelven diferentes subtareas a la vez, y una vez que se obtienen todas las respuestas, se puede componer la solución.

Implantación de nuevas versiones de software
--------------------------------

Lucho mucho con esto en todas partes.

La forma en que se maneja normalmente el desarrollo de software es que se tiene alguna versión estable que se ejecuta en producción para los usuarios, luego se tiene algún entorno de prueba donde se hace el nuevo desarrollo, y de vez en cuando se mueven las novedades fuera del entorno de prueba para la producción, lo que se llama un lanzamiento. Este proceso es relativamente seguro y lento.

Desde que he pasado gradualmente a una arquitectura micro-frontend, en la que el frontend de la aplicación (lo que ve el usuario) está completamente separado del backend (donde se produce el cálculo y el procesamiento de datos), el proceso de liberación puede funcionar de forma diferente.

Todavía tengo un entorno de producción que siempre funciona. A partir de ella, se crea una nueva rama para que cada tarea aborde una característica específica. Como uso Vercel (un muy buen proveedor en la nube) para alojar el frontend, puedo tener una URL personalizada para cada rama de desarrollo.

Una vez probada una nueva función, se fusiona inmediatamente y se despliega en producción en un proceso asíncrono. Por lo tanto, puede ocurrir comúnmente que haya tal vez 15 despliegues de nuevas versiones cada día.

Mucho de esto se relaciona con una idea que me transmitieron en el LMC: "Una función que proporcionas a un cliente mañana, aunque sea en un estado imperfecto, tiene mucho más valor para él que una función que está perfectamente depurada pero que no estará disponible hasta dentro de un año". Pero esto sólo puede funcionar si se dispone de una forma de distribuir las noticias con rapidez, que suele ser el desarrollo web.

Lo que me gusta mucho del framework Next.js (construido sobre Recto) y Vercel es el principio de que conectas tu repositorio de GitHub directamente a Vercel, y con cada commit hay una construcción automática, pruebas, y luego despliegue a producción cuando todo está bien. Así, el desarrollador no tiene que preocuparse de nada, y la aplicación se despliega cada hora con cero esfuerzo. Es muy importante formalizar las cosas rutinarias y luego automatizarlas.

Publicación de contenidos
----------------

He desglosado más alto docenas de temas y mensajes conmigo en Apple Notes. Para cada tema, se me ocurren continuamente nuevas ideas que anotar y clasificar por etiquetas. Cuando se generan varias notas para un tema, las convierto en capítulos, y luego convierto el grupo de capítulos en artículos y posts de FB.

Características de este principio:

- No sé de antemano cuántos artículos voy a publicar,
- Pero también sé que puede haber muchas,
- Puedo asegurar que cada post está lleno de información, ideas y conocimientos que han madurado con el tiempo (este post tardó aproximadamente 3 meses en madurar),
- Es sostenible y lo disfrutaré porque no tengo que escribir una tonelada de texto en un momento dado y arriesgarme a olvidar algo.

Y luego el proceso de publicación.

Publico muchos contenidos de forma discreta en la web primero, para que Google se dé cuenta primero y la página sea indexada. Cuando tiene una buena cobertura de palabras clave y estoy contento con él en general, el tema va a las redes sociales, por ejemplo.

Con muchos temas, sé que no serán de interés y es más probable que te molesten. Pero al mismo tiempo, quiero tenerlos en línea porque alguien puede buscarlos en el futuro. En ese caso, el artículo se queda sólo en la web. Un ejemplo de estos artículos son los diversos artículos superpuestos, que enlazan toda el área temática para que tenga el mayor número de temas posible en el sitio. Los artículos de portada suelen ser más técnicos y aburridos. O son categorías generadas por la máquina en las que simplemente organizo múltiples artículos en temas, cubriendo así el mayor número posible de palabras clave, que luego alguien puede querer buscar.

Conocimiento, educación, pruebas
------------------------------

Me gusta jugar con tecnologías en las que no está claro desde el principio para qué servirá un día.

Como la traducción automática. A primera vista, no tiene sentido traducir decenas de artículos a idiomas extranjeros para lectores con los que no estoy en contacto. Por otro lado, puede ser uno de los pasos para tomar una decisión en el futuro para la que hay que cumplir los requisitos previos.

En general, nadie sabe cómo será el futuro. Así que para mí tiene mucho sentido abarcar el mayor número posible de posibilidades que quieras entender, al menos superficialmente, y quizás abordar en el futuro. Es bueno pensar en los pasos no sólo como una tarea resuelta, sino como un proceso evolutivo interminable que no tiene destino final.

¿Funciona este enfoque para entregar proyectos a medida?
--------------------------------------------------------

La mayoría de las veces, no.

Tienes que tener un cliente que sea tu socio y ambos queráis ofrecer la mejor solución posible, aunque sepas de antemano que no vas a saber cuál es hasta el final.

En nuestro negocio se llama desarrollo ágil, y estos principios se basan en él. Por otra parte, he observado que el desarrollo ágil, tal y como lo conozco, no me conviene directamente, y me gusta idear alternativas dobladas.

Con la agilidad, veo mucho del principio de compromiso en términos de quién resuelve qué dentro de un sprint particular. Para mí tiene más sentido construir el entorno de forma que se tenga una enorme acumulación de tareas que se resuelvan en función de lo que más se necesite en ese momento, o de lo que yo tenga la capacidad mental de hacer. Cuando estoy de viaje, por ejemplo, tiendo a abordar tareas de pensamiento más desarrolladas que puedo hacer fuera sin un ordenador. Cuando vuelvo a estar en la oficina, me ocupo de tareas más intensivas en algoritmos y matemáticas porque puedo concentrarme completamente e invertir mucha energía mental.

No recomiendo este principio si estás montando una tienda electrónica nueva o si tu negocio está en quiebra. Necesitas un entorno estable que funcione por sí mismo, y sólo estarás haciendo joyas. Pero al mismo tiempo, sabes que si no haces nada, tu entorno estable se acabará algún día.
