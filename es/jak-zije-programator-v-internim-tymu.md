Cómo vive un programador en un equipo de desarrollo interno
===========================================================

> id: '9309b0f2-0265-43aa-a9c3-10b2d486cdfc'
> slug:
> 	cs: jak-zije-programator-v-internim-tymu
> 	es: como-vive-un-programador-en-un-equipo-de-desarrollo-interno
> 
> publicationDate: '2022-01-10 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: e65dd78fa9813b552f30b98f04aacb30

La honestidad tiene un alto precio.

Este sitio siempre ha sido una descripción de la realidad que vive la gente de la informática, así que me gustaría analizar mi experiencia trabajando en equipos de desarrollo. Las siguientes son experiencias generales que he tenido en distintas empresas. Ninguna experiencia está asociada a una empresa en particular y no sirve necesariamente como crítica.

Las empresas tienden a no querer personas trabajadoras y proactivas
----------------------------------------------

¿Tienes muchas ideas? ¿Quiere innovar? ¿Disfrutas encontrando soluciones elegantes a los complejos problemas que resuelve tu equipo y que asolan a media empresa? ¿Es consciente de la importancia de la seguridad, el diseño del software y la búsqueda de cuellos de botella en los proyectos?

Probablemente serás infeliz en el equipo de desarrollo durante mucho tiempo.

El trabajo en equipo es algo con lo que he estado luchando mucho últimamente -específicamente estoy nadando en él y tratando de entender los principios completamente ininteligibles para mí para adherirme.

- Trabajar en equipo significa desarrollar una solución que todo el equipo entienda. A menudo no es lo mejor. Casi nunca es elegante. De hecho, siempre es ineficiente. Pero tiene la gran ventaja de que todo el equipo lo entiende y se puede gestionar a largo plazo. Esta es una idea muy importante. Porque con los grandes proyectos no hay tanta presión para ser eficiente porque como empresa puedes escalar a través de la gente y tienes suficiente dinero. Es mucho más importante entregar las cosas a tiempo, aunque estén parcialmente rotas y con limitaciones desconocidas hasta ahora, pero a tiempo. Tiene que ver con la idea de que la solución que entregues mañana (aunque sea imperfecta) tiene mucho más valor para el cliente que una solución 100% depurada que no existirá hasta dentro de un año.
- Innovar y sacar las cosas adelante es más bien un error. Tiene que ver con el hecho de que hay personas reales que trabajan en las empresas, que tienen familia y que sólo quieren trabajar en horario laboral. De ello se deduce necesariamente que la gente tiene un tiempo limitado para aprender y probar cosas nuevas. Esencialmente, las empresas tienen que meter en las horas de trabajo de las personas la formación y el desarrollo que pueden utilizar para el trabajo futuro. Una consecuencia interesante es entonces el aplazamiento de las decisiones y el aprendizaje de cosas nuevas al tiempo necesario. Por mi parte, entiendo este planteamiento y me parece que tiene mucho sentido. Si tuviera empleados, también preferiría personas tranquilas que duraran en la empresa, por ejemplo, 15 años, antes que personas que tienen una gran visión general y quieren seguir adelante, pero será a costa del equipo. Es cierto que la solución colectiva nunca será igual que la individual, pero eso no significa que sea peor.

Las personas como yo son una fuente potencial de problemas y conflictos. Es genial pensarlo así.

Cuando revises el código, sólo busca errores objetivos
----------------------------------------

Cada empresa tiene sus normas de estilo de código. Desgraciadamente. Me molestó mucho al principio.

Cuando se utiliza uno de los lenguajes más maduros, como .NET, las reglas de estilo de código tienden a ser más similares. Sólo entonces te das cuenta de que PHP y JavaScript siguen existiendo en el mundo. Especialmente lo de PHP es un poco frustrante a veces. PHP es un lenguaje muy maduro con muchas y muy buenas características, pero según mi experiencia, se utiliza en las empresas a un tercio de su potencial total. Las razones suelen ser variadas, la mayoría de las veces por inercia.

En este sentido, hace tiempo que busco una solución procedimental para la revisión del código. Llevo mucho tiempo jugando con PhpStan y siguiendo las ideas en torno a él. La idea básica detrás de PhpStan es que sólo resalta los errores objetivos que seguramente ocurrirán en algún momento. Cuanto más exploro PhpStan y lo llevo al siguiente nivel, más valido internamente lo cierto que es.

Estaría bien que PhpStan se implantara en al menos la mitad de las empresas checas. Sería aún mejor si al menos llegara al nivel 6. En la práctica, he experimentado que las mejores empresas tienen el nivel 4 o 5.

Precisamente el otro día me preguntaba si puede haber una aplicación de producción real en funcionamiento que tenga su propio equipo de desarrollo, y que ni siquiera pase el nivel 0, que es el que controla las cosas que se esperan en cualquier aplicación. Algo así como asegurarse de que todas las clases y funciones existen, los archivos no tienen errores de análisis, etc. Y sí, hay aplicaciones de este tipo. Pero la situación está mejorando a largo plazo. Ojalá.

Así que sigo un enfoque similar con codereview. Sólo informo de las cosas que objetivamente son siempre erróneas. A menudo me encuentro con errores de apreciación del grado: algo va mal, pero, de nuevo, no es un problema tan grande como para tener que abordarlo. Muchos problemas se resuelven por convención, o declarando que el riesgo es pequeño, por lo que no tiene sentido tratarlo.

Siempre he considerado que la falta de tipos de datos (incluso los compuestos) es un error crítico. Entonces comprendí que existe la prueba y el volcado.

Creo que no tengo una conclusión concreta para esta parte. Sólo tengo un montón de comentarios subjetivos al respecto, que es más probable que sean una fuente de malentendidos mutuos, así que no los publicaré. A largo plazo, me inclino por la conclusión de que no puedo hacer codereview: o soy demasiado estricto o demasiado benévolo. No puedo decir a nivel general lo que realmente importa y lo que no es tan importante. Me interesaría bastante saber cómo lo hacen los demás. Nadie ha sido capaz aún de darme una respuesta que me sirva también.

No hay dinero en el desarrollo a medida.
---------------------------------

¿Tiene una agencia y hace desarrollo web a medida? Si no eres lo suficientemente grande y haces grandes proyectos para otras empresas, probablemente no existirás ningún día.

La razón de que esto ocurra es la escasa financiación de los proyectos a medida. Probablemente tenga que ver con la naturaleza de los contratos, que son más rentables para los compradores. De hecho, la mayoría de las agencias están brutalmente infrafinanciadas y sólo obtienen un beneficio real para los propietarios.

Cuando desarrollas internamente un proyecto como empresa para ti mismo, un retraso en la entrega de un mes probablemente no importa tanto. Como agencia, si no entregas un trabajo antes de un mes, tienes garantizado que ese mes estarás durmiendo lentamente en el trabajo, arruinando constantemente tu salud, el cliente maldiciendo al teléfono y los tickets, siempre preguntando si podría ser más rápido, y como recompensa seguirás pagando una penalización contractual. Y si no lo haces, el cliente te etiquetará como un contratista poco fiable y puede plantearse ir a otro sitio, donde de todas formas no será mejor.

Pero esas son las reglas del juego, que he llegado a conocer en la práctica, y que han hecho un agujero en mi presupuesto.

La contratación es probablemente la forma de negocio más compleja en el ámbito de las TI. No quieres hacer contrataciones. La contratación te destruirá. Te llamarán el fin de semana, por la noche, en Navidad. La mitad del trabajo que haces no te lo pagan. Te disculparás por errores que no puedes cometer. Estarás mirando en Instagram el storytelling de los amigos que se han ido de vacaciones por tercera vez este año mientras tú te sientas en tu oficina a las 3 de la mañana de un sábado terminando un proyecto de un cliente que de todas formas te regañarán el lunes porque había más en el contrato de lo que podías hacer. La gente se tomará vacaciones y bajas por enfermedad y tú trabajarás en su lugar. O te despiden. Y entonces estarás contento de pagar el alquiler.

Pero además, se aprende mucho. Porque el hecho de estar bajo presión te obliga a aprender y ser eficiente para que la próxima vez puedas hacer un poco más en el mismo tiempo. Lo malo es que no puedes aplicar realmente esos conocimientos y experiencia en otros lugares porque a las empresas no les interesa (ya lo he explicado antes).

Afortunadamente, esta es una historia de 2019 y está muy lejos.

Nunca habrá un mundo ideal
-------------------------

¿Si hago lo que me gusta? ¿Y tú? :D

Supongo que todo es una cuestión de proporción. Probablemente nunca harás un trabajo que te satisfaga al 100%. Probablemente siempre habrá alguien que te explique que no puedes hacer aquello que llevas 10 años aprendiendo a la fuerza y tú sabes internamente que puedes.

Por otro lado, por una cierta negación de tu propia personalidad, ganas el espacio para tener algo más. Como tiempo de fin de semana, mejor salud, más tiempo para ti, un entorno estable, etc. Que no se puede sostener a largo plazo también es obvio.

Me gusta poder probar cosas diferentes para experimentar de primera mano cómo lo hacen otros. Trabajar en un equipo de desarrollo es un gran aburrimiento comparado con un trabajo a medida.

Ya no se trata de encontrar la mejor y más rápida solución, sino de colaborar. Se trata de mejorar la comunicación, las emociones y, sobre todo, de aprender a ser humano. Y eso también es un gran valor.
