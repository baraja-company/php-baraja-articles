Por qué y cómo utilizar marcos y bibliotecas
============================================

> id: '5ea6cdde-f67c-4f3f-8871-37b27ee8e23a'
> slug:
> 	cs: proc-pouzivat-frameworky
> 	es: por-que-y-como-utilizar-marcos-y-bibliotecas
> 
> publicationDate: '2020-02-16 22:13:46'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '0926b7ec4f59904c008388431ff22eb5'

Hay un chiste famoso que dice que los programadores empiezan a utilizar frameworks sólo cuando escriben el suyo propio y descubren que no va a ninguna parte. Lo curioso de esto es que es cierto. Yo mismo lo he experimentado. Dos veces, incluso.

Sin embargo, incluso en la página principal de <a href="https://nette.org">Nette</a> dice:

> Los verdaderos programadores **no utilizan frameworks**. Escriben aplicaciones web a través de la línea de comandos directamente al servidor. Esto es un homenaje a ellos. Para el resto de nosotros, Nette hará nuestro trabajo mucho más fácil y agradable.

El papel de los frameworks en el desarrollo de aplicaciones
-----------------------------------

Estoy seguro de que tú mismo lo sabes:

Tienes una idea para un gran proyecto, así que empiezas a escribir código greenfield directamente en PHP puro... en pocas horas te das cuenta de que gran parte del trabajo sigue siendo repetitivo y estás resolviendo problemas básicos del sistema. Por ejemplo, cómo conectarse a una base de datos, cómo generar un formulario, cómo validar datos, cómo enviar un correo electrónico y mucho más.

Probablemente escribirás tus propias funciones para llamar a estas tareas. Si estás escribiendo varios proyectos a la vez, empezarás a compartir estas funciones entre proyectos en un gran archivo desordenado, y las irás ajustando continuamente según las necesites.

Y cuando las funciones están en una cierta etapa de desarrollo que se empieza a construir un nuevo proyecto en la parte superior de ellos cada vez y desarrollar directamente, entonces se puede hablar de haber escrito su propio marco, o al menos una biblioteca.

¿Qué es y qué hace un marco
-------------------------

Como ya hemos mostrado con un ejemplo práctico de la vida, un framework es una colección de muchas funciones (pero idealmente clases y objetos) que ahorran el trabajo del programador. Cuando desarrolla, no tiene que pensar en cómo conectarse a una base de datos, sino que sabe que ya está programado en alguna parte y "simplemente funciona".

Los frameworks terminados son, por tanto, el acervo de cientos de personas que han desarrollado miles de aplicaciones durante un largo periodo de tiempo para depurar una solución y un ecosistema para abordar nuevos proyectos.

Con los frameworks, también se obtiene un conjunto de **mejores prácticas**, que son formas de resolver problemas sin tener que pensar en ello. Si te encuentras con un problema, es probable que alguien lo haya resuelto antes que tú, y siempre es mejor encontrar una solución en la documentación que tener que resolverla tú mismo de forma complicada.

La programación abstracta y el principio de encapsulación
---------------------------------------------

Los frameworks bien diseñados como Nette permiten programar a un **nivel de abstracción muy alto**.

De esta manera, el programador (el usuario del framework) no tiene que entender lo que ocurre internamente y cómo funcionan exactamente los componentes, lo que ahorra mucho tiempo y esfuerzo. Puede centrarse en resolver los problemas reales de su aplicación y obtener resultados muy rápidamente.

El propio David Grudl (el autor del framework Nette) dijo una vez que diseñó Nette para poder programar un sitio web para un cliente en una noche, cuando llega a casa tarde del pub y tiene que lanzar el sitio web por la mañana. Esto se debe principalmente a que el desarrollador está completamente alejado de la parte técnica.

Para que esto funcione y para que el desarrollador acabe de utilizar el framework terminado en su totalidad, es muy importante utilizar correctamente el principio de <a href="/encapsulación">encapsulación</a>.
