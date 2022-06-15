Compositor: resumen completo de las funciones avanzadas
=======================================================

> id: a74d8d59-91ce-4602-ad52-80cf89a647bd
> slug:
> 	cs: composer
> 	es: compositor-resumen-completo-de-las-funciones-avanzadas
> 
> perex:
> 	- Composer je pokročilý správce balíků a závislostí pro vaše PHP aplikace. Článek popisuje jeho všechny výhody a možnosti použití.
> 	- Composer es un gestor avanzado de paquetes y dependencias para sus aplicaciones PHP. Este artículo describe todas sus ventajas y usos.
> 
> publicationDate: '2020-03-10 20:18:19'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '68340d4b4d3c8a6ed143ede176fbf04e'

Como ya sabes, [Composer](https://getcomposer.org/) es un robusto gestor de paquetes y dependencias para PHP, a través del cual puedes gestionar elegantemente cientos de proyectos a la vez y distribuir el código una vez escrito a todas las aplicaciones simultáneamente.

Este tutorial sirve de guía detallada para el desarrollador. Cubriremos todas las técnicas avanzadas importantes para trabajar con Composer, además de explicar los detalles técnicos y las dependencias relevantes.

Instalación de Composer
-------------------

Independientemente de la plataforma, lo descargamos desde [el sitio web oficial de Composer](https://getcomposer.org/).

Internamente, se descarga el archivo PHP `composer-setup.php`, que cuando se ejecuta en modo CLI puede instalar Composer. También es importante saber que Composer no funciona sin PHP, así que primero verifique que tiene PHP ejecutándose en su computadora (sólo necesita estar disponible para la Terminal).

En Mac y Linux, Composer funciona inmediatamente después de la instalación y basta con llamar al comando `composer -v` para verificar rápidamente que Composer está instalado correctamente.

En Linux, se puede utilizar el siguiente comando para instalar: `/usr/bin/php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"`

En Windows, es una buena idea instalar la herramienta [Git Bash para Windows](https://gitforwindows.org/), que permite abrir un Terminal específico que se comporta casi como Linux y permite trabajar en el mismo entorno que en Linux.

La instalación para los servidores es la misma que en un entorno local. Sólo asegúrese de que tiene la versión correcta de PHP que Composer utiliza internamente.

Comandos disponibles
----------------

Composer implementa una serie de comandos.

El uso es: `composer <comando>`, por ejemplo: `composer update`.

Resumen para `1.10.0`:

| Comando | Descripción / Significado |
|-----------------------|----------------|
| Muestra una breve información sobre Composer.
| `archive` | Crea un archivo con el contenido del paquete de Composer seleccionado. |
| Abre la página de inicio del paquete seleccionado, autor u otra página relacionada en un navegador web. A menudo puede contener documentación sobre cómo utilizarlo. |
| `cc` | Borra la caché interna de Composer con versiones de paquetes descargados en el pasado. |
| Comprueba si se cumplen los requisitos de instalación para la plataforma actual.
| Borrar el caché interno de Composer.
| Borrar la caché interna de Composer.
| | `config` | Establece una directiva de configuración.
| Crea un nuevo proyecto basado en el paquete seleccionado y crea automáticamente una carpeta para colocar el proyecto.
| Muestra los paquetes que han provocado la instalación del paquete seleccionado.
| Diagnostica el sistema para identificar errores comunes. El procesamiento de la salida depende del desarrollador, es sólo un listado. |
| `dump-autoload` | Genera un nuevo <a href="/autoloading-trid">autoloader</a>.
| `dumpautoload` | Genera un nuevo <a href="/autoloading-trid">autoloader</a>. |
| `exec` | Ejecuta los binarios y scripts de Vendor. |
| `fund` | Explora cómo hacer/modificar sus dependencias. |
| `global` | Permite ejecutar comandos globales de Composer desde la variable `$COMPOSER_HOME`. |
| Imprime la ayuda de los comandos.
| `home` | Abre la página de inicio de un paquete específico en el navegador. |
| `i` | Instala todas las dependencias del proyecto según el archivo `composer.lock`, si existe y es válido. Si se produce un problema, se utiliza la información de `composer.json` y `composer.lock` se revierte a su estado original. |
| `info` | Muestra información sobre los paquetes actualmente instalados en el proyecto. Muestra los nombres de todos los paquetes, sus versiones actuales y una breve descripción. |
| `init` | Crea una función básica `composer.json` en el directorio actual. |
| Instala todas las dependencias del proyecto según el archivo `composer.lock`, si existe y es válido. Si se produce un problema, se utiliza la información de `composer.json` y `composer.lock` se revierte a su estado original. |
| Muestra una lista de todos los paquetes, sus versiones y la licencia actual.
| Muestra una lista de comandos disponibles.
| Muestra una lista de todos los paquetes para los que hay una versión más reciente disponible para su instalación y que cumplen con las dependencias. Para cada paquete, se mostrará la última versión compatible que Composer sugiere para su instalación. |
| Muestra qué paquetes y dependencias impiden la instalación del paquete o versión solicitada.
| `remove` | Elimina el paquete de la sección de configuración `require` o `require-dev`. |
| Añade el paquete solicitado a `composer.json` y lo instala. Si las dependencias no pueden ser satisfechas, se volverá al estado original. |
| `run` | Ejecuta los scripts definidos en `composer.json`. |
| Ejecuta los scripts definidos en `composer.json`.
| `search` | Busca paquetes por palabra clave o consulta de búsqueda. |
| Actualiza el archivo interno `composer.phar` a la última versión.
| Actualiza el archivo interno `composer.phar` a la última versión.
| Muestra información detallada sobre los paquetes actualmente instalados.
| `status` | Muestra un resumen de los cambios locales realizados en los paquetes de forma manual, basándose en una comparación con el origen del paquete desde el que se instaló originalmente. |
| Muestra sugerencias de paquetes. Las sugerencias pueden incluir varios tipos de acciones, como la instalación de actualizaciones de seguridad, etc. |
| `update` | Actualiza todo el proyecto de acuerdo a las dependencias, para que siempre sean todas satisfechas por `composer.json`. Si tiene éxito, actualiza `composer.lock`, donde escribe las versiones actualmente instaladas. |
| Alias a "actualizar".
| Alias a "actualizar".
| Comprueba si hay errores de sintaxis en `composer.json` y `composer.lock`.
| Muestra los paquetes que han provocado la instalación del paquete actualmente seleccionado, incluyendo todas las dependencias.
| Muestra los paquetes y versiones que impiden la instalación del paquete o versión seleccionados.

Creación y definición de un proyecto
----------------------------------

Cada proyecto gestionado por Composer está definido por un archivo `composer.json` en su raíz que define todas las dependencias. El archivo puede crearse manualmente para un proyecto existente o automáticamente al crear un proyecto.

Dado que todo en Composer es un paquete, el propio proyecto puede basarse en un paquete. Así, por ejemplo, si estás creando docenas o cientos de proyectos muy similares, tiene sentido poner su configuración y estructura básica en un paquete separado en el que basar la instalación.

Un ejemplo de este tipo de paquetes es mi [Baraja Sandbox](https://github.com/baraja-core/sandbox), que se basa en Nette 3.0 puro y añade una dependencia básica a mi [Package Manager](https://github.com/baraja-core/package-manager), que utilizo para todos los proyectos y la gestión de dependencias en la configuración de Nette.

La caja de arena se instala entonces simplemente con el comando

```shell
$ composer create-project baraja/sandbox <your-project-name>
```

Basándose en el nombre del proyecto, Composer creará automáticamente una carpeta para instalar el proyecto (copiar el contenido del paquete e instalar las dependencias).

En la carpeta `vendor`, Composer gestiona entonces todos los paquetes (sus archivos físicos están allí) y genera una autocarga de clases, que idealmente ponemos directamente en `index.php` como una línea:

```php
require __DIR__ . '/vendedor/autoload.php';

// El propio código de la aplicación
```

Instalación de paquetes y dependencias adicionales
-------------------------------------

Dentro de un proyecto funcional podemos instalar nuevos paquetes y añadir dependencias muy fácilmente.

Hay dos maneras de hacerlo:

- Con el comando `composer require ...`,
- Añadiendo una dependencia directamente al archivo `composer.json` en la sección `require`, y luego utilizando el comando `composer update`.

Prueba a instalar, por ejemplo, PackageManager: `composer require baraja-core/package-manager`, o [Doctrine](https://github.com/baraja-core/doctrine): `composer require baraja-core/doctrine`.

Si no se puede instalar el paquete elegido, se pueden pedir razones específicas y Composer enumerará las dependencias que lo impiden. A menudo basta con arreglar una dependencia de una versión concreta o eliminar el código incompatible. Para obtener información detallada, utilice el comando: `composer why baraja-core/doctrine`.

Actualización de proyectos y paquetes
-----------------------------

Un proyecto bien diseñado está desarrollado para que puedas descargar fácilmente las actualizaciones a lo largo del tiempo y tener siempre las últimas versiones de todos los paquetes. La principal ventaja es que obtienes todos los errores corregidos, a menudo mejoras de rendimiento y un montón de nuevas características. Además, el cambio gradual hará que la actualización sea menos complicada después de mucho tiempo, ya que se solucionarán los problemas sobre la marcha a menor escala y se evitarán las incompatibilidades.

Para actualizar todos los paquetes y dependencias, utilice el comando `composer update`.

El propio proceso de actualización puede fallar en algunos casos. La razón suele ser una dependencia rota o un paquete incompatible.

Para obtener información detallada sobre por qué no se puede instalar un paquete, utilice el comando: `composer why-not baraja-core/doctrine`. Si ya tenemos el paquete y sólo la versión concreta no funciona (no quiere instalarse), también podemos pedir la versión concreta: `composer why-not baraja-core/doctrine:v1.0.20`.

Dentro del archivo `composer.json`, también podemos listar la dependencia de un tiempo de ejecución específico. Esto es especialmente útil cuando estamos desarrollando un proyecto con varias personas y queremos verificar que tienen todas las extensiones instaladas.

Normalmente, se comprueba la versión de PHP (debe ser `7.1.0` o posterior):

```json
{
   "require": {
      "php": ">=7.1.0"
   }
}
```

Posiblemente otras extensiones del sistema:

```json
{
   "require": {
      "php": ">=7.1.0",
      "ext-json": "*",
      "ext-session": "*",
      "ext-PDO": "*"
   }
}
```

Estas reglas se tienen en cuenta a la hora de instalar paquetes o actualizaciones. Esto ayuda a evitar problemas que se harían evidentes en el momento de la ejecución. Normalmente, por ejemplo, un paquete de pasarela de pago necesita comunicarse con la API, por lo que debe admitir dependencias de las extensiones `curl` y `json`.

Solución de problemas de dependencia
-----------------------------

A menudo, las violaciones de dependencia ocurren debido a una mala versión de PHP. En este caso Composer lanza un mensaje por ejemplo `su versión de PHP (7.3.11) anulada por la versión "config.platform.php" (7.1) no satisface ese requisito.`.

Muy a menudo el error es causado por la configuración directamente en el proyecto `composer.json`, donde está la siguiente sección:

```json
"config": {
   "platform": {
      "php": "7.2"
   }
}
```

El cambio **debe hacerse directamente en el archivo**. En el caso de un proyecto global (antes de la instalación o con una dependencia global), puede forzar la versión de Composer con `composer config -g platform.php 7.2.14` (el interruptor `-g` significa `global`).

En algunos casos, queremos instalar las últimas versiones de los paquetes e ignorar la configuración del entorno local. En este caso, podemos utilizar el comando avanzado:

```shell
$ composer update --ignore-platform-reqs
```

**Utiliza el parámetro `ignore-platform-reqs` bajo tu propio riesgo, ya que puede dañar el proyecto.

Llamadas manuales de Composer, parámetros y gestión de la memoria
------------------------------------------------------

Composer es en realidad un script PHP envuelto en un llamado PHAR, que es una versión compilada de una aplicación PHP. Conocer esta información puede servir para, por ejemplo, parametrizar mejor la propia llamada.

En proyectos realmente grandes, a veces sucede que nos quedamos sin RAM y necesitamos asignar mucho más, o aumentar el tiempo de procesamiento de los scripts.

Por ejemplo, en Windows puede utilizar este comando para ello:

```shell
$ php -d memory_limit=-1 C:/xampp/htdocs/composer.phar update
```

El interruptor `memory_limit=-1` le dice a Composer que no sea susceptible al límite de RAM y que consuma toda la memoria que necesite.

Scripts de usuario personalizados tras la acción de Composer
--------------------------------------------

Después de ejecutar las acciones de Composer, puede invocar la ejecución automática de scripts definidos por el usuario que realicen acciones específicas en el proyecto o, por ejemplo, generen una configuración después del despliegue. Principalmente utilizo este enfoque en un servidor local para ofrecer una herramienta de configuración automática de la base de datos, generar un esquema de base de datos, etc.

Registramos los scripts necesarios en la sección `scripts` de `composer.json`:

```json
"scripts": {
   "post-autoload-dump": "Baraja\\PackageManager\\PackageRegistrator::composerPostAutoloadDump"
}
```

En este caso se llama automáticamente al método estático `composerPostAutoloadDump` de la clase `Baraja\PackageManager\PackageRegistrator`. Composer tiene esta clase disponible porque primero realizó la generación de las clases <a href="/autoloading-trid">autoloader</a>.

Si sólo queremos ejecutar los scripts y no realizar acciones innecesarias con Composer, el comando `composer dump` es muy útil, ya que sólo genera un nuevo autoloader (que recomiendo mantener siempre actualizado) y luego ejecuta los scripts inmediatamente. Si quieres probar a usar scripts, he preparado un paquete listo [Baraja PackageManager](https://github.com/baraja-core/package-manager) que implementa scripts inteligentes e interfaz interactiva para Nette framework.

¿Vendedor de versiones en Git?
------------------------

Una cuestión que discutimos a menudo con los desarrolladores es si hay que versionar el contenido de la carpeta `vendor` en Git, o hacer que se vuelva a generar en cada instalación.

En general, la solución más limpia parece ser **no versionar** el contenido en absoluto e instalar todo cada vez. Pero la realidad es que de vez en cuando un desarrollador deja de desarrollar un paquete, o lo elimina por completo. La descarga constante de paquetes también complica las instalaciones y actualizaciones locales, y también ralentiza los despliegues, y a veces puede causar breves cortes del sitio cuando las nuevas versiones de paquetes se descargan incorrectamente.

Veo la suspensión del vendedor como una forma de "seguridad". Si los archivos están físicamente en el sistema de versiones, tenemos al menos una garantía rudimentaria en el servidor de producción de que los paquetes funcionarán y su código es exactamente igual que en la instalación que se ejecuta localmente. Además, Vendor suele ocupar sólo unidades de MB, lo que sin duda vale la pena para asegurar un sitio de trabajo dadas las capacidades de disco actuales.

**Nota práctica:** Para el sitio medio que mantengo, `vendor` no ocupa más de `30 MB`, que es una capacidad aceptable para Git. Al clonar un repositorio con junior, no tenemos que ocuparnos de enseñarle cómo poner en marcha el sitio, y simplemente "funciona de inmediato".

Paquetes personalizados de Composer
-----------------------

Puedes crear tus propios paquetes dentro de Composer, ya sean públicos (registrados en [Packagist](https://packagist.org/)) o privados (debes tener tu propio servidor de paquetes, por ejemplo [Satis](https://getcomposer.org/doc/articles/handling-private-packages.md)).

El tema de la creación, el mantenimiento, el desarrollo y el versionado de paquetes es muy complejo y será objeto de otro artículo.

Mientras tanto, puede leer el artículo [Semantic Versioning](https://semver.org/lang/cs/), que he traducido para usted.
