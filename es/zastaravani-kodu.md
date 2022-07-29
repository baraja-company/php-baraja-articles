Obsolescencia del código: cómo mantener la compatibilidad
=========================================================

> id: '7837ee7e-a150-47bf-a338-2f4c03d5bbed'
> slug:
> 	cs: zastaravani-kodu
> 	es: obsolescencia-del-codigo-como-mantener-la-compatibilidad
> 
> publicationDate: '2022-07-29 17:10:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '727b60c7258fb70e21b5acb445404db0'

En el desarrollo de grandes sistemas (por ejemplo, aplicaciones empresariales, paquetes de software compartidos, bibliotecas, ...) en los que varias capas y desarrolladores se comunican entre sí, surge el problema de cómo gestionar la publicación de nuevas versiones de código.

Veamos una situación de ejemplo en la que queremos desarrollar un paquete de Composer compartido para una comunidad de desarrolladores.

Versiones semánticas
--------------------

Antes de resolver el problema de la compatibilidad hacia atrás y hacia delante, tenemos que averiguar cómo hacer un seguimiento de los cambios en el software. Actualmente (2022), la mejor manera de versionar todos los cambios es en Git. El repositorio de software puede compartirse, por ejemplo, a través de GitHub o GitLab. Cada cambio de software tiene un identificador único que identifica cada confirmación y describe lo que realmente ha ocurrido.

La siguiente estrategia me ha funcionado bien a la hora de desarrollar bibliotecas:

Al principio del desarrollo, se crea un commit inicial en la rama `master` (o `main`), donde se confirma la estructura de archivos subyacente.

Para cada nueva solicitud, se crea una rama independiente de la maestra en la que trabajar. Cuando el cambio está listo, se envía una solicitud de fusión al maestro en forma de `Pull request`. Se realiza una revisión del código sobre la solicitud y, si todo está bien, el cambio se fusiona con el maestro.

Si la rama contiene un cambio incompatible hacia atrás (BC break, de `Back Compatibility Break`), debe marcarse en consecuencia. El método para marcar las pausas de BC se discute en los siguientes capítulos.

La versión de producción de la biblioteca se etiqueta con etiquetas que tienen la siguiente estructura (basada en **Semantic Versioning 2.0.0**):

Escribimos el número de versión en el formato `MAJOR.MINOR.PATCH`. El incremento de los números de versión se realiza de la siguiente manera:

- `MAJOR` - cuando hay un cambio que no es compatible con otros (API)
- `MINOR` - cuando se añade funcionalidad manteniendo la compatibilidad con versiones anteriores
- `PATCH` - cuando se corrige un error y se mantiene la compatibilidad con versiones anteriores

Mediante el uso de versiones previas y la adición de metadatos es posible afinar la información. Por ejemplo: `1.0.0-alpha`, `1.0.1-beta+2`.

Puede leer más sobre el versionado semántico en el sitio web oficial: https://semver.org.

Compatibilidad con el pasado y con el presente
-------------------------------

Cuando se diseña un software, siempre hay que pensar en la **compatibilidad hacia atrás** (las nuevas funciones y cambios deben ser compatibles con el código antiguo) y, en algunos casos, en la **compatibilidad hacia delante** (las funciones actuales deben ser compatibles con los futuros cambios de la interfaz).

Hacer bien ambas tareas es un gran reto. No siempre es posible realizar un cambio sin romper la compatibilidad.

Al realizar modificaciones, siempre es necesario proceder por pasos y dar a los usuarios el tiempo suficiente para reaccionar a los cambios.

Las siguientes secciones describen cómo pensar en esto.

Etapa 1: Marcar una característica como obsoleta
--------------------------------------

El tipo básico de amenaza de compatibilidad es la eliminación o el cambio de nombre de una característica que existía en el pasado. La mayoría de las veces esto se debe a que los argumentos que acepta la función han cambiado, o se trata de una lógica antigua que debe ser manejada de manera diferente en la nueva forma.

En la primera etapa, las partes antiguas del código deben marcarse como obsoletas, pero no se modifican de ninguna manera.

En PHP hay una anotación `@deprecated` para esto, que debe ser escrita directamente encima de los métodos, funciones, propiedades, variables, constantes, y en general todo el código obsoleto.

También es una buena práctica escribir la razón por la que una cosa en particular está obsoleta y cómo se cambiará en el futuro. Por ejemplo, dar el nombre de una nueva función o método de uso.

Un ejemplo del mundo real de marcar el código como obsoleto: Las constantes serán eliminadas, es mejor utilizar el Enum incorporado (BC break debido a la migración a una nueva versión de PHP):

```php
class OrderNotification
{
	/** @deprecated since 2022-05-24, use enum OrderNotificationType */
	public const
		TYPE_EMAIL = 'correo electrónico',
		TYPE_SMS = 'texto';
```

La anotación `@deprecated` sólo causará una advertencia silenciosa para el IDE (herramienta de desarrollo) y las herramientas de compilación. No rompe nada.

Fase 2: Llamar a un nuevo método/lógico
--------------------------------------

En la segunda fase, sustituimos la implementación antigua por la nueva, pero utilizamos el nuevo método en la implementación antigua. Esto ayudará a mantener la compatibilidad de la interfaz sin que el usuario lo note.

Ejemplo: el método está obsoleto porque en su lugar se ha creado un nuevo servicio estático. Como alguien puede utilizarla, simplemente se marca como obsoleta y llama internamente a la nueva implementación. Por lo general, el promotor puede asumir que el método se eliminará por completo en el futuro.

```php
/** @deprecated since 2021-09-11 use Ip::get() instead. */
public static function userIp(): string
{
	return Ip::get();
}
```

Fase 3: Cambiar las anotaciones para el análisis estático
-------------------------------------------

Si está utilizando un análisis estático como PhpStan (¡muy recomendable!), es una buena idea reescribir primero las anotaciones de PHPDoc antes de cambiar los tipos de datos. El análisis estático notificará al usuario que algo está roto, pero el tiempo de ejecución permanecerá intacto.

Etapa 4: Tirar el aviso
-----------------------

En la cuarta fase, se llama a un nuevo método y al mismo tiempo se lanza un error de nivel de nota. La aplicación sigue funcionando, sólo empieza a almacenar gradualmente información en el registro del sistema de que una función está obsoleta y será cambiada o eliminada. A partir de ahora, alertaremos activamente sobre este tipo de cambios. El desarrollador verá errores durante el desarrollo o la compilación.

```php
/** @deprecated since 2021-05-01, use UserMetaManager en su lugar. */
public function getMeta(int $userId, string $key): ?string
{
	trigger_error(__METHOD__ . 'Este método está obsoleto, utilice UserMetaManager en su lugar.');
	return $this->userMetaManager->get($userId, $key);
}
```

Etapa 5: Lanzar una excepción
------------------------

Recomiendo lanzar una de las excepciones fatales antes de eliminar completamente el método. Esto es especialmente importante porque la aplicación se detendrá por completo y el error no podrá ser ignorado. A diferencia de eliminar el código por completo, el usuario será notificado de lo que realmente ha sucedido y podrá solucionar el error fácilmente.

Etapa 6: Eliminación completa del código
-----------------------------

En la última etapa, se eliminará por completo el código antiguo. Si algún usuario no ha arreglado las dependencias, su aplicación estará rota.

Las rupturas graves de BC en áreas sensibles deben hacerse siempre en la siguiente versión `MAJOR` y deben señalarse al menos una versión `MAJOR` antes lanzando un aviso. Si no lo hace, la actualización de la biblioteca será extremadamente difícil.
