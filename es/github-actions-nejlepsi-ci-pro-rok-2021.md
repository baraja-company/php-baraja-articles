Acciones de GitHub: el mejor CI para 2021
=========================================

> id: c1fe3b77-a506-4f20-bf1e-90ee82817c7d
> slug:
> 	cs: github-actions-nejlepsi-ci-pro-rok-2021
> 	es: acciones-de-github-el-mejor-ci-para-2021
> 
> perex:
> 	- 'Protože už nějaký čas vyvíjíte webové aplikace, určitě jste si všimli, že se pro vás celá řada věcí rutinně opakuje, i když by nemusela. Velmi často jde o technickou správu projektů, verzování souborů, automatická kontrola kódu, zpracování různých dat, nebo třeba deploy na server a bezpečnostní věci kolem toho.'
> 	- 'Como ya llevas un tiempo desarrollando aplicaciones web, probablemente te habrás dado cuenta de que muchas cosas te resultan rutinarias y repetitivas, aunque no deberían serlo. Muy a menudo se trata de la gestión de proyectos técnicos, el versionado de archivos, la revisión automatizada de código, el procesamiento de datos diversos, o tal vez el despliegue en el servidor y las cuestiones de seguridad en torno a eso.'
> 
> publicationDate: '2021-02-07 15:46:39'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: d3d151927a02dde744d1202449c904dc

Como ya llevas un tiempo desarrollando aplicaciones web, probablemente te habrás dado cuenta de que muchas cosas te resultan rutinarias y repetitivas, aunque no deberían serlo. Muy a menudo se trata de la gestión de proyectos técnicos, el versionado de archivos, la revisión automatizada de código, el procesamiento de datos diversos, o tal vez el despliegue en el servidor y las cuestiones de seguridad en torno a eso.

En la consultoría de empresas, me encuentro muy a menudo con el problema de que se subestima mucho la prevención. La razón suele ser que los desarrolladores perciben algunas cosas como muy desafiantes y que les supondrán más trabajo. Pero lo cierto es que, por lo general, sólo hay que configurar todo el montaje una vez y luego sólo hay que cosechar los beneficios a largo plazo.

Qué es la integración continua (CI)
----------

CI/CD es una herramienta que permite automatizar las tareas rutinarias que tienen una base similar y se repiten en los proyectos. El uso más común de la IC es la ejecución de pruebas automatizadas, la revisión de código, los despliegues automatizados (el despliegue de una aplicación en un servidor web), la gestión de proyectos o quizás las auditorías de seguridad.

Piense en CI como un sistema operativo virtual que ejecuta comandos predefinidos en el Terminal o ejecuta programas específicos. El resultado de CI es un éxito o un fracaso, que se muestra directamente en el proyecto, pero también se envía a los administradores que pueden reaccionar ante él. Una buena práctica es ejecutar trabajos de CI después de un evento específico (por ejemplo, una confirmación en el repositorio, una solicitud de fusión/retirada recibida, una etiqueta/versión/liberación, o una ejecución de cron timeloop).

Cómo funciona específicamente la IC
------------

La base de CI es un archivo de configuración donde definimos su comportamiento y respuesta a los eventos. En el caso de GitHub, basta con crear un directorio de ayuda `.github/workflows` y crear cualquier archivo con la extensión `.yml` dentro. GitHub lo analizará con cada confirmación y lo ejecutará cuando sea necesario.

Imaginemos que queremos definir un nuevo archivo de configuración con una tarea específica. Dado que esta es una tarea que será ejecutada directamente por GitHub u otro entorno en la máquina virtual, necesitamos definir cosas básicas sobre el entorno, incluyendo

- El nombre de la tarea (por ejemplo, el nombre de la prueba)
- Eventos de activación (en qué evento debe activarse la tarea, por ejemplo, cada vez que se envíe al repositorio o se reciba una solicitud de extracción)
- La definición de las propias tareas (puede haber muchas tareas ejecutándose en paralelo)

En el caso de los puestos de trabajo, definimos además:

- Nombre del trabajo
- Entorno de ejecución (normalmente el nombre del sistema operativo)
- Pasos para construir el contenedor

El contenedor anterior es ya la primera preparación para **la dockerización completa del proyecto**. El uso básico de CI es relativamente fácil y no es necesario entender Docker en absoluto, sólo conocer los comandos en Terminal.

Como parte de los pasos específicos de la tarea, necesitamos ejecutar los comandos individuales que construirán la instalación del sistema operativo que acabamos de instalar. Así que en el caso de PHP, será importante instalar sólo el lenguaje PHP (y especificar la versión), clonar el repositorio del proyecto en el corredor, instalar las dependencias del proyecto (más a menudo [Composer](/composer)), y ejecutar la prueba en sí.

¿Por qué utilizar las acciones de GitHub (CI)?
--------

Hay una superstición común en la comunidad de TI de que Microsoft es la corporación malvada que hace cosas de baja calidad. Sin embargo, en el caso de las acciones de GitHub, esto no es cierto en absoluto, porque tienen, objetivamente, la mejor plataforma de CI del mundo.

La ventaja fundamental de GitHub CI es la simplicidad y la elegancia de la notación. Con unas pocas líneas, puede configurar su propio entorno de pruebas y gestionarlo automáticamente, incluso sin conocimientos previos. Al mismo tiempo, considero que la velocidad de procesamiento de tareas específicas es una gran ventaja. Por ejemplo, una prueba sobre codestyle (formato de código) y PhpStan (comprobación de tipos de datos, lógica y corrección general) puede ser evaluada por GitHub Actions en un proyecto normal en menos de 50 segundos, mientras que GitLab, por ejemplo, evalúa la misma prueba en casi 8 minutos. Otras soluciones como Travis son relativamente caras y, de todos modos, la mayoría de los desarrolladores no aprecian las ventajas de sus características especiales.

Acciones preparadas
-----

GitHub cuenta con una gran base de datos de acciones y paquetes listos para usar de inmediato. Suelen ser sistemas operativos, lenguajes de programación o bibliotecas de la comunidad.

Puedes encontrar una base de datos de acciones justo en el [Marketplace](https://github.com/marketplace?type=actions), por ejemplo mi acción favorita es [Enviar SMS en caso de fallo a través de Twillio](https://github.com/marketplace/actions/twilio-sms).

Ejemplos terminados
------

Hay muchos ejemplos [que publico en mis propios repositorios](https://github.com/baraja-core) en los que se puede ver de forma transparente la configuración específica que utilizo.

Por ejemplo, en febrero de 2021 utilicé esta configuración para la comprobación del estilo de código en un proyecto:

```yaml
name: Coding Style

on:
  push:
    branches:
      - master
  pull_request:
    types: [ assigned, opened, synchronize, reopened ]
  schedule:
    - cron: '1 * * * *'

jobs:
  nette_cc:
    name: Nette Code Checker
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 7.4
          coverage: none

      - run: composer create-project nette/code-checker temp/code-checker ^3 --no-progress
      - run: php temp/code-checker/code-checker --strict-types --no-progress

  nette_cs:
    name: Nette Coding Standard
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 8.0
          coverage: none

      - run: composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
      - run: php temp/coding-standard/ecs check src
```

Este archivo de configuración instala la última versión de Ubuntu (`runs-on: ubuntu-latest`), clona el repositorio del proyecto (`uses: actions/checkout@v2`), instala PHP (`uses: shivammathur/setup-php@v2`) versión 7.4 (`php-version: 7.4`), instala el paquete code-checker, y luego ejecuta el trabajo vía PHP.

Algunas notas más para los usuarios que no tienen tanta experiencia:

- CI iniciará una máquina virtual con un sistema operativo completo cada vez que se ejecute, que se puede utilizar para llamar a los comandos en la Terminal al igual que, por ejemplo, su servidor
- Para comprobar los resultados de las pruebas, se utilizan los llamados códigos de retorno definidos por el propio Linux. En concreto, cuando un programa devuelve `0` (cero), significa éxito y cualquier otro número significa fracaso. Por ejemplo, los scripts de shell funcionan de la misma manera
- Cuando queramos escribir una prueba personalizada o una lógica más compleja, podemos usar PHP, por ejemplo, y ejecutarlo como un trabajo CLI (el comando `php file.php`), que se ejecutará y podrá hacer todo lo que tenga derecho a hacer (trabajar con archivos, comunicarse por internet, salir por pantalla, ...)
- Cuando CI se ejecuta, toda la salida se registra en la pantalla y se puede ver directamente en el navegador web
- CI no es interactivo, aunque Terminal puede ejecutar operaciones interactivas con el usuario, CI no lo soporta y debe ser diseñado como una tarea que a veces se inicia y a veces termina
- Se define un tiempo de espera de 1 hora para que el contenedor CI se ejecute, si no se procesa todo en ese tiempo (por ejemplo, el programa se detiene), todo el sistema se terminará forzosamente y se informará de un error
- GitHub puede ejecutar trabajos CI automáticamente mediante cron, pero muy a menudo no los ejecuta en el momento adecuado, sino que los ejecuta con un retraso
- También puede envolver toda la lógica de CI en un contenedor Docker y ejecutarlo localmente en todas las máquinas o en un servidor, por ejemplo
- Si necesitas acceder a información secreta (por ejemplo, la contraseña de la base de datos), hay una opción para definir variables secretas directamente en GitHub y CI las tiene disponibles en tiempo de ejecución
- El despliegue en un servidor web siempre es mejor hacerlo a través de SSH
- El tiempo total de ejecución de los trabajos de CI está limitado, normalmente a 2 mil minutos al mes (muy a menudo esto será suficiente para ti, yo nunca lo he agotado porque un trabajo se ejecuta durante un minuto como máximo), o puedes aumentar el tiempo por una tarifa
- También puedes alojar corredores de CI en tu servidor y saltarte el límite de minutos, pero muy probablemente tu servidor no será mucho más rápido porque Microsoft ha invertido mucho en su Azure donde se aloja GitHub Actions y se ejecuta en servidores muy potentes
- Muy a menudo es útil instalar paquetes específicos sólo para CI (típicamente PhpStan y varias características de seguridad), así que ponlos en la sección `composer.json` del archivo `require-dev` para que no se instalen en un proyecto específico como una dependencia obligatoria

Aplicaciones y bots de GitHub
-----

A diferencia de otros hosts de repositorios, GitHub soporta las llamadas GitHub Apps y los bots de automatización. Se trata de una gran base de datos de bots preparados que pueden realizar operaciones automatizadas en tus proyectos (incluso sin CI) y que, cuando encuentran algo, por ejemplo, archivan una incidencia o envían un pull request con una corrección.

Yo personalmente lo uso y lo recomiendo:

- Dependabot](https://dependabot.com) - comprobación automática de dependencias en Composer, por ejemplo, cuando sale una nueva versión de los paquetes que usas y es compatible, envía automáticamente un pull request
- Codecov](https://github.com/marketplace/codecov) - comprueba la cobertura del código con pruebas automáticas y grafica qué partes del código no han sido cubiertas todavía
- Snyk](https://github.com/marketplace/snyk) - comprueba automáticamente las vulnerabilidades de seguridad
- Imgbot](https://github.com/apps/imgbot) - optimización automática del tamaño de las imágenes

Se pueden encontrar muchas otras aplicaciones en el [Marketplace](https://github.com/marketplace?type=apps).

Más consejos y trucos prácticos
-----

Me he aficionado a utilizar las tareas de automatización en GitHub.

Tengo comprobaciones automatizadas en todos mis repositorios, así que cuando envío cualquier commit (o cualquier otra persona de la comunidad), obtengo información en tiempo real sobre si se ha violado la calidad del código (codestyle y PhpStan), la seguridad, etc.

Tengo algunas pruebas que se ejecutan automáticamente cada hora. Por ejemplo, si hay una vulnerabilidad de seguridad o un CVE, lo sabré en una hora como máximo, incluso a nivel de proyectos concretos y qué ha pasado exactamente.

Con el tiempo, después de probar diferentes configuraciones, he llegado a la conclusión de que considero que las siguientes dependencias de Composer son la configuración mínima ideal:

- `phpstan/phpstan` - comprobación de la corrección y la lógica del código
- `tracy/tracy` - informe y registro de errores de alto nivel
- `phpstan/phpstan-nette` - Extensiones y especialidades de Phpstan en Nette
- `spaze/phpstan-disallowed-calls` - una brillante librería de Michal Spacek que maneja el (des)uso de cosas peligrosas en el código (desde volcados de variables olvidadas hasta la posibilidad de ejecutar una cadena de usuario como un comando del shell)
- `roave/security-advisories` - comprueba la instalación de versiones no seguras de los paquetes (si se encuentra una vulnerabilidad de seguridad en un paquete concreto o se publica un CVE, este paquete impedirá la instalación de la versión corrupta)

Lo ideal es tener la configuración de seguridad básica configurada para que se ejecute automáticamente al menos cada 8 horas y que se envíen los errores a su correo electrónico. Porque cada vez que falla la instalación de un proyecto o se descubre una nueva vulnerabilidad, quiero saberlo cuanto antes y reaccionar de inmediato.

Recuerda que todo funciona de forma automática, por lo que siempre puedes estar seguro de que, aunque utilices procesos complicados para comprobar la seguridad y otras cosas, no te cuesta ningún esfuerzo extra y sólo necesitas una configuración inicial bien ejecutada.

Porque la prevención y la automatización tienen un enorme potencial.
