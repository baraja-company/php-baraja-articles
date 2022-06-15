Refactorización de un proyecto PHP heredado: cómo ponerse al día con la deuda tecnológica
=========================================================================================

> id: '8f37305a-e9dd-4b1e-9f53-04f0fca648f7'
> slug:
> 	cs: refaktoring-legacy-php-projektu
> 	es: refactorizacion-de-un-proyecto-php-heredado-como-ponerse-al-dia-con-la-deuda-tecnologica
> 
> publicationDate: '2021-05-11 20:45:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: b13673b23a86bc82a88636e4a9464b4b

Cuando consulto a propietarios de proyectos con conocimientos y experiencia, a menudo me encuentro con la cuestión de la sostenibilidad a largo plazo de un proyecto digital. Muchos de los grandes proyectos que superan los 3 años de desarrollo empiezan a quedarse obsoletos internamente y dejan de ser sostenibles. Imagínese ahora un equipo de desarrolladores con distintos niveles de conocimiento, experiencia y, sobre todo, diligencia.

Para mantener su proyecto digital en el nivel más alto técnicamente, necesita:

- **Desarrolladores competentes y un director de proyecto** que tengan una gran comunicación y que todos sepan lo que están haciendo y cómo repercute en los demás,
- **Herramientas funcionales y flujo de trabajo** que todo el equipo conoce y adapta su trabajo a los demás en consecuencia,
- **Tareas automatizadas** que proporcionan una rutina diaria que es tan fácil de olvidar que es extremadamente importante.

Si está desarrollando un proyecto de gran envergadura, simplemente no lo tiene fácil. Es importante hacer las cosas bien, pero es más importante hacer las cosas correctas.

¿Qué es la refactorización y por qué es necesaria?
---------------------------------------

El concepto de refactorización engloba un conjunto de actividades que modifican la apariencia y la lógica interna del código de un programa sin afectar al entorno. Puedes pensar en el proceso de refactorización como una limpieza navideña: sigue siendo lo mismo, pero está mucho más organizado y se desechan las cosas innecesarias. Con la refactorización, también es importante tener en cuenta que no se trata de un evento único, sino de un proceso a largo plazo que hay que repetir en ciclos regulares. Si no lo haces, te esperan todo tipo de errores extraños, pérdidas económicas, problemas de incorporación y lloros.

Si consigues mantener el proyecto estable y actualizado, obtendrás grandes beneficios:

- El proyecto será **más seguro**. Regularmente se publican parches para errores y vulnerabilidades de seguridad en las bibliotecas que se utilizan. Debe abordar la seguridad, es una de las funciones vitales básicas de un proyecto saludable.
- El código y la arquitectura interna serán más sencillos y estarán mejor pensados. Podrás encontrar mejor los fallos, e incluso muchos de ellos se pueden prevenir.
- Con un código simplificado, también puedes incorporar desarrolladores junior para <a href="https://www.youtube.com/watch?v=1wZtdIb-Ppk">reducir significativamente</a> tu presupuesto global.
- Es posible que descubras que estás haciendo algunas cosas demasiado complejas y complicadas: el proyecto se simplificará en general.

Para mantener el rumbo de un gran proyecto digital, hay que correr. Pero tú quieres crecer.

Cómo refactorizar con seguridad
-------------------------

Cada refactorización es una gran apuesta que puede no resultar rentable.

Siempre me aseguro de tener un entorno estable mucho antes de embarcarme en la refactorización.

Para la estabilidad, se necesita especialmente el **registro de errores funcionales**. Para proyectos sencillos, basta con <a href="https://tracy.nette.org">Tracy</a>, que registra los errores en un archivo HTML. Para proyectos más avanzados, entran en juego herramientas como <a href="https://sentry.io/welcome/">Sentry</a> o <a href="https://rollbar.com">Rollbar</a>. Personalmente utilizo Tracy en todos los proyectos, otras herramientas dependiendo del tipo de proyecto.

Aproximadamente un mes antes de la refactorización, empiezo a hacer un seguimiento de los errores y creo una hoja de cálculo con los errores conocidos para centrarme en ellos más adelante. Siempre es importante saber qué errores se han introducido con la refactorización y cuáles ya contenía el proyecto en el pasado. Esto ayudará a defender mejor los resultados del trabajo ante el cliente.

Una vez que sé, gracias al registro, que estoy trabajando en un entorno relativamente estable, podemos ponernos manos a la obra. Otros epígrafes incluyen descripciones de los métodos según el riesgo que se esconde tras ellos.

Fijar el estilo y la norma de codificación
-----------------------------------

La mayoría de los proyectos no tienen un estilo de formato de código consistente. Esto es un gran error. Un proyecto bien escrito parece el de una persona, en el que todo está escrito igual.

Los errores básicos de formato son bien corregidos por la herramienta <a href="https://doc.nette.org/en/3.1/code-checker">Nette Code Checker</a> que paso regularmente por todo el proyecto.

El estilo de codificación, por su parte, abarca la forma de formatear el código y la sangría de las distintas expresiones del lenguaje. El estándar <a href="https://www.php-fig.org/psr/psr-12/">PSR-12</a> se utiliza mucho en el mundo de PHP, y muchos otros estándares se basan en él. Recomiendo usar.

Algunos proyectos combinan torpemente <a href="/espacios-y-pestañas">pestañas y espacios</a>. Para evitar eficazmente que se rompa la convención de los espacios en blanco (y otros errores), cree un archivo `.editorconfig` en la raíz del proyecto que le diga a su editor cómo formatear el código recién escrito.

Personalmente utilizo esta configuración:

```txt
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.{php,phpt}]
indent_style = tab
indent_size = 4
```

Además, arregla todas las <a href="/apóstrofes-y-notas-de-correo">notas-de-correo a apóstrofes</a>. Se puede hacer de forma automática.

También puedes comprobar el formato de tu código de forma automática, he preparado una <a href="https://github.com/baraja-core/sandbox/blob/0db1f41068b6cedf79500c6a8b0ac26eae94a8eb/.github/workflows/coding-style.yml">demostración totalmente funcional en GitHub</a> para ello. Si también necesita autocorregir el código, puede hacerlo con la misma herramienta. PhpStorm también es muy bueno en la fijación automática de código directamente, incluso en masa sobre todo el proyecto.

Cómo arreglar el estilo de codificación de los grandes proyectos
----------------------------------------------

Para proyectos grandes, te recomiendo que hagas el formateo automático del código de un módulo a la vez, ya que esto puede crear un gran número de conflictos en Git. Por lo tanto, la mejor estrategia es arreglar tantas líneas como sea posible con un solo commit grande, empujar ese commit directamente al maestro, y luego distribuir el cambio a las otras ramas.

Si los conflictos son muy grandes, tiene sentido formatear el código en el maestro primero, y luego otra vez en cada rama grande donde hay muchos cambios. Muchas líneas cambiadas se anulan entre sí (haz un avance rápido), por lo que resolverás muy pocos conflictos, o a veces ninguno.

Si no haces nada, el código seguirá siendo malo. Las reescrituras iterativas a lo largo de muchos años definitivamente no funcionan, porque cada fusión introduce la necesidad de arreglar docenas de líneas en las que no se ha producido ningún cambio, y complica innecesariamente tanto la revisión del código como las posibles reversiones de los cambios.

PhpStan - análisis de código estático y corrección de errores de tipo
------------------------------------------------------

Antes de embarcarse en un arreglo lógico a gran escala, es muy importante revisar y arreglar las **características básicas del ciclo de vida del proyecto**. Se trata de cosas tan obvias que se esperan de cualquier proyecto, pero que a veces no se cumplen.

Por ejemplo:

- Todas las clases, métodos y funciones deben existir
- La herencia de clases no debe romperse
- Las clases deben implementar la interfaz o la interfaz ancestral utilizada. Al mismo tiempo, no debe heredar las clases "finales".
- No debe llamar a funciones no seguras como `eval()`, `shell_exec()`, `var_dump()` y demás. Y si los llama de todos modos, debe indicarse explícitamente en el comentario
- Siempre hay que atrapar las excepciones y no dejar que toda la aplicación se caiga

La solución a este problema es instalar <a href="https://phpstan.org">PhpStan</a> en el proyecto y arreglarlo **al menos al nivel 1**. Sí, es difícil, y sí, es mucho trabajo. Pero si no lo haces, cada refactorización se convierte en una ruleta rusa, y el desarrollador sólo espera que el daño sea el mínimo posible.

El nivel base de PhpStan no requiere que los tipos de datos y otras cosas formales existan en todas partes, por ejemplo, lo que trataremos en el futuro. Por otro lado, puede darse el caso de que una función o método devuelva un determinado tipo de datos pero afirme otra cosa en el typehint.

Rector - reparación iterativa segura
-----------------------------------

Un conocido dictado de la programación dice que antes de añadir un nuevo error al sistema (como lanzar una excepción), debemos añadir primero ese error como advertencia, y sólo entonces convertirlo en un error fatal si no se manifiesta durante mucho tiempo.

Lo mismo ocurre con los tipos de datos. Cuando refactorizo código desconocido, dejo que herramientas automáticas como <a href="https://getrector.org">Rector</a> añadan anotaciones de comentario al código primero, lo que no rompe nada, pero ayuda a clarificar lo que estará donde después. Estos comentarios son escuchados por PhpStan, que puede ser usado para verificar con seguridad que no hemos roto nada y que es una modificación segura.

Generalmente añado comentarios a las propiedades y argumentos en un commit, luego espero quizás un mes, y cuando todo funciona a largo plazo, recurro a reescribir a tipos de datos fijos en lugares donde no ha habido problemas a largo plazo.

Véase el artículo <a href="https://tomasvotruba.com/blog/2019/07/29/how-we-completed-thousands-of-missing-var-annotations-in-a-day/">Cómo completamos miles de anotaciones @var que faltaban en un día</a>.

Para más información sobre <a href="https://github.com/rectorphp/rector">Rector, consulte GitHub</a>.

Actualización de dependencias
----------------------

Antes de entrar a actualizar las dependencias de los paquetes o incluso las versiones de PHP, es importante investigar y aprender a utilizar <a href="/github-actions-best-ci-for-2021">pruebas automatizadas</a>.

Si las pruebas funcionan, podemos ir a la herramienta <a href="/composer">Composer</a> para realizar la actualización. Si puedes, solicita la ayuda de un robot <a href="https://dependabot.com">Dependabot</a> que actualice el `composer.json` de tu proyecto automáticamente y pueda comprobar los cambios de compatibilidad.

Si tiene un gran conjunto de cambios por venir, hágalos siempre lentamente e increméntelos versión por versión. Nunca actualice muchos paquetes a la vez. Después de cada actualización, escanee todo el proyecto con PhpStan y corrija los errores. Es un proceso largo que dura varias horas, pero hay mucho en juego.

Dónde ir ahora
-------

Los siguientes pasos son individuales en función del tipo y el estado del proyecto. En general, el código bien diseñado y escrito por desarrolladores senior se mantiene órdenes de magnitud mejor que el código comprado a bajo precio a un junior que trabajaba en una agencia de medios que tiene el desarrollo web más bien como una actividad secundaria, por así decirlo.

Crucemos los dedos. Va a ser duro, pero lo superarás.
