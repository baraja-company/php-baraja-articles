Lo que cambiaría de Nette si fuera David Grudl
==============================================

> id: bad8fe4f-92a4-4396-8698-7ec42bb2aef8
> slug:
> 	- null
> 	es: lo-que-cambiaria-de-nette-si-fuera-david-grudl
> 
> cs: co-bych-zmenil-na-nette
> publicationDate: '2022-11-18 17:45:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '953e6c78b9dff185bbf8e3e9582d24c3'

Llevo casi 6 años utilizando activamente Nette Framework para el desarrollo de software comercial. Durante los primeros años estaba muy entusiasmado y el marco de trabajo respondía perfectamente a todas las necesidades que teníamos como equipo, y básicamente no había razón para buscar otra herramienta.

Desde aproximadamente 2019, estoy empezando a ver cierta caída del entusiasmo original dentro de Nette, donde empiezo a echar de menos algunas de las características avanzadas. A menudo se trata de cosas que van más allá del propio marco de trabajo, por lo que ni siquiera espero que se implementen nunca, pero, por otro lado, un desarrollador tiene que tomar una decisión sobre cómo continuar con el desarrollo en adelante, y tal vez optar por una tecnología diferente.

Capa de la base de datos
-----------------

Considero que la base de datos Nette es uno de los mayores errores del marco, por desgracia.

Todas las semanas tenemos entrevistas en la empresa en las que jóvenes recién salidos de la escuela, o incluso personas de nivel intermedio que están en transición desde otro marco, solicitan y descubren Nette Database como parte de su exploración de Nette. Como capa de base de datos, no está tan mal, pero el problema entonces es el uso práctico en proyectos reales.

Esto se debe a que Nette Database no se esfuerza en presionar a los desarrolladores para que utilicen tipos de datos estrictos desde el principio, para que nombren las columnas y las relaciones, para que separen los métodos de consulta (repositorio) de los modelos, y otros pequeños matices que hacen mucho en general.

Durante muchos años utilicé la biblioteca Dibi, que desempeñó un gran papel en su día. Permitía consultar la base de datos con bastante facilidad y unificaba la interfaz. Por otro lado, los tiempos han avanzado, y cuando las bases de datos devuelven objetos no tipados, PHPStan no estará contento sólo por principio.

Personalmente, yo incorporaría el soporte nativo para entidades y repositorios en Nette Database, o proporcionaría soporte oficial para Doctrine dentro de Nette. Si se programan aplicaciones al nivel de una gran empresa, hay que tomarse en serio el desarrollo, y Doctrine en particular tiene mucho sentido. Al mismo tiempo, se quiere educar a los recién llegados en una mejor tecnología de inmediato, y no se quiere enseñarles viejos hábitos.

Registro de errores y tácticas
---------------------

Sigo considerando que Tracy es el mejor depurador en tiempo de ejecución para PHP.

Cuando llegué a una agencia digital sin nombre en 2017, y vi el estado técnico de sus proyectos Nette, me horroricé. Cuántas veces tuvieron sitios escritos en puro PHP sin ningún intento de registrar los errores. Una solución bastante fácil era instalar Tracy en el proyecto, llamar a `Debugger::enable()` y no hacer nada más. Los fallos se registraban automáticamente en un directorio que sólo había que recoger manualmente cada pocos días.

En cuanto a Tracy, me parece que es un producto acabado que David tiene pocos motivos para mejorar. Por otro lado, sigo viendo margen de mejora en una nueva dirección.

Por ejemplo, ¿por qué Tracy no incluye funciones de pago para empresas o particulares que se toman en serio la seguridad?

Tracy podría implementar una interfaz web personalizada del tipo Sentry en la que se crea una cuenta y los errores de producción se envían automáticamente a través de una API en la que se puede hacer un seguimiento de los proyectos. La aplicación también podría solicitar sitios individuales, y detectar automáticamente que no está disponible, y notificar al propietario de otras maneras (ya que Tracy no puede detectar lógicamente una caída del servidor). También me encuentro a veces con el problema de que Tracy se activa accidentalmente en un sitio de producción, y se puede averiguar a través de DIC cómo acceder a la base de datos o en otro lugar. Tracy debería alertarte de esto también.

Tracy también podría implementar otras pequeñas cosas que conozco de Node.js. Por ejemplo, para la visualización del código fuente, podría ser posible elegir el intervalo de caracteres en el que se resalta la línea de manera especial, por lo que la posibilidad de resaltar un token específico dentro de una línea. Al mismo tiempo, estaría bien añadir soporte para una segunda línea de resaltado (quizás azul) para mostrar el contexto en la siguiente parte de la aplicación.

Cuando se muestra un fragmento de código fuente, no me importaría añadir un botón que pueda renderizar el resto del código en ajax para poder explorarlo directamente en el navegador desde la pila de llamadas. Esto es lo que hace Symfony, y es una gran característica. Si esto se complementara con un análisis estático básico con la capacidad de rastrear métodos, clases y herencias, se ahorraría mucho trabajo de depuración para todo el equipo.

Si la pila de llamadas está atravesando un paquete, sería bueno mostrar la versión del paquete con la que estoy trabajando para un archivo en particular. A menudo me sale un error en un paquete que estoy desarrollando, y también podría saber visualmente que es una versión antigua.

Enrutamiento estático
----------------

Dentro de Nette Router, permitiría definir un nuevo tipo llamado router estático. Sería un descendiente del enrutador básico, pero sólo podría aceptar una cadena-literal, que no sería una expresión regular, sino un camino recto. Muchas páginas funcionan de forma estática (contactos, varios artículos, sorprendentemente a menudo incluso la página de inicio), y el enrutador tiene que evaluar las reglas regex para la coincidencia y la composición de la URL cada vez debido a esto.

Si se utilizara una ruta estática en una plantilla Latte, por ejemplo, podría traducirse de forma segura a una cadena estática `{$basePath}/path` en tiempo de compilación de la plantilla, por lo que no habría necesidad de compilar las 100 URLs que hay en el diseño, por ejemplo, en cada petición.

Entiendo que puedo conseguir la misma funcionalidad almacenando en caché la plantilla, pero apreciaría mucho más una solución de sistema.

En el marco de Next.js, caí completamente en el concepto de enrutamiento estático. No existe `n:href`, en definitiva hay que conocer la URL de antemano, lo que obliga a no hacer tantas redirecciones, pensar en los formatos de las URL de antemano, y mantenerlas persistentes.

Interfaz PSR
------------

Me parece una pena que Nette no implemente de forma nativa la interfaz PSR. Como desarrollador de paquetes, esto te lleva por el camino equivocado si quieres definir una dependencia de Composer en Nette. Por un lado, Nette te soluciona muchos problemas, pero por otro lado sabes que después no lo vas a sustituir. Más de una vez me he encontrado prefiriendo usar Symfony, Laravel, o una interfaz genérica completamente diferente debido a la falta de una interfaz genérica.

La falta de apoyo a los estándares genéricos para la portabilidad futura es la principal razón por la que me alejé completamente de Nette en 2022 y desarrollamos nuevas aplicaciones en equipo exclusivamente en genérico.

Capa API
----------

¿Y si quiere utilizar Nette como backend para gestionar las peticiones de la API REST? ¿Construye un presentador y envía la respuesta como `$this->sendJson()`? ¿Instalará una biblioteca de terceros? ¿O es usted David Grudl, que resuelve la necesidad de una biblioteca perdida escribiendo una nueva de inmediato?

¿Por qué Nette no puede implementar algo tan común como la API REST desde el principio? Hoy en día, casi todas las aplicaciones necesitan comunicarse a través de una API, por ejemplo, para proporcionar datos al frontend.

Para los recién llegados, esto les lleva de nuevo a utilizar el Latte y los fragmentos incorporados en lugar de descubrir que hay una opción mejor, y es construir todo el frontend por separado junto, y sólo proporcionarle datos.

Confiar en lo que ocurrirá dentro de 10 años
-------------------------

El último punto es muy pragmático y proviene de un debate que escuchamos de vez en cuando en los departamentos de negocio del equipo.

¿Qué pasaría si David Grudl dejara de desarrollar Nette por completo? ¿Y si alguien dejara de desarrollarlo? ¿A qué cambiaríamos? ¿Y qué reto supondría eso?

A muchos desarrolladores (a menudo sin experiencia empresarial) les gusta burlarse de esta preocupación, diciendo que no pasa nada. Como si, pero no lo es.

Cuando te enfrentas a la cuestión de si invertir tiempo en Nette o en React y Node.js para formar a los desarrolladores junior de tu empresa, lamentablemente gana la última opción.

Nette aún no ha perdido el tren, pero en las docenas de entrevistas que he tenido en los últimos seis meses, lo noto bastante.
