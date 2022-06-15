Serie Doctrina - Introducción
=============================

> id: fbd0461a-53fe-4713-8926-82e31bd1fb9b
> slug:
> 	cs: doctrine-uvod
> 	es: serie-doctrina-introduccion
> 
> publicationDate: '2021-08-27 11:40:00'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: '2bc62308fcb66acda5a09ac95b5eb2c2'

Doctrine es una librería avanzada de PHP para trabajar con bases de datos orientadas a objetos. El principal propósito y objetivo de Doctrine es describir el esquema de la base de datos mediante entidades de datos y manipular los datos de forma totalmente orientada a objetos.

Este paradigma se denomina ORM (Object-relational mapping), que es [design-pattern](/design-patterns) para convertir (envolver) los datos almacenados en una base de datos relacional en un objeto que pueda utilizarse en un lenguaje orientado a objetos. Por lo tanto, para entender y utilizar Doctrine, debe conocer al menos los fundamentos de la [Programación Orientada a Objetos](/oop).

¿Por qué aprender la doctrina?
------------------------

Hay muchas razones:

- Doctrine es la base de datos ORM más extendida, utilizada por la mayor parte de la comunidad avanzada de PHP.
- Simplificará drásticamente el diseño de su aplicación PHP
- Proporciona una forma coherente de diseñar, versionar, transferir y hacer copias de seguridad del esquema de la base de datos
- Puedes [obtener muchas tablas de la base de datos descargando el paquete](https://github.com/baraja-core/shop-product) sin tener que averiguar y configurar nada
- Las relaciones entre tablas se convierten en entidades físicas reales
- Las salidas de la base de datos no serán matrices ordinarias no tipificadas, sino que obtendrá objetos físicos reales
- Se obtiene una forma fácil de realizar muchas operaciones simultáneamente dentro de una sola transacción
- Aumentará fácilmente la seguridad y la resistencia de las aplicaciones simplemente sabiendo cuándo ocurre lo que ocurre, y que ocurre de forma segura
- Obtendrá una capa de código y base de datos fácilmente comprobable
- Descubrirá todo un ecosistema en torno a Doctrine que resuelve muchos problemas con elegancia. A menudo encontrará soluciones sencillas a problemas complejos que de otro modo serían casi imposibles de resolver fácilmente
- Aprenderás muchas cosas nuevas, explorarás nuevas ideas y utilizarás la base de datos en todo su potencial
- Deshágase de las complejas consultas SQL. Doctrine proporciona una interfaz de escritura de consultas personalizadas (DQL) que es muy potente
- Las aplicaciones serán más rápidas. Descubrirá fácilmente el espacio para la optimización de la aplicación, aprovechará el lazy loading y encontrará los cuellos de botella de la aplicación

La opinión del autor de este artículo ([Jan Barasek](https://baraja.cz)) es que Doctrine es la mejor manera de trabajar con una base de datos PHP. Simplemente no tiene competencia.

¿Cómo empezar?
----------

Antes de empezar a utilizar Doctrine en su totalidad, es necesario preparar un entorno adecuado. Si estás empezando con PHP o no tienes conocimientos avanzados, la mejor opción es instalar el Nette Framework con el paquete de extensión [Doctrina Baraja](https://github.com/baraja-core/doctrine), que integra automáticamente el soporte completo. Primero descargue el paquete a través de [Composer](/composer), luego configure la extensión DI y Doctrine comenzará a funcionar automáticamente.

Para que Doctrine funcione correctamente, es necesario preparar una base de datos vacía (Doctrine puede trabajar con un proyecto existente, pero para los primeros pasos esto es inapropiado ya que se corre el riesgo de sobrescribir los datos existentes) y configurar la conexión. Dado que Doctrine no es sólo una biblioteca de base de datos, sino que proporciona un marco de trabajo de base de datos avanzado, es necesario [resolver otra configuración](/configurar-conexiones-con-baraja-doctrine). La mayoría de las configuraciones se sobrescriben automáticamente en ese paquete para Nette, sin embargo en la configuración mínima su servidor debe soportar las extensiones `APCu Cache` o `SQLite3`.

Si todo se ha configurado correctamente, se creará un nuevo servicio DI `Baraja\Doctrine\EntityManager` en Nette, que podrá [inyectar](https://doc.nette.org/cs/3.1/di-usage) en Presenter:

```php
namespace App\FrontModule\Presenters;

use Baraja\Doctrine\EntityManager;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Si consigues inyectar el servicio básico de EntityManager, puedes empezar a aprender y trabajar con Doctrine.

¿Cómo proceder?
--------

Los siguientes capítulos son una combinación de una guía de referencia de la tecnología Doctrine, años de experiencia, patrones de diseño y soluciones preparadas. Juntos recorreremos todos los elementos básicos de Doctrine, desde la definición de su propia entidad, hasta la generación de un esquema de base de datos física, pasando por el trabajo con una herramienta de versionado y el despliegue en producción.

Llevo mucho tiempo utilizando Doctrine y he resuelto miles de casos con él. Mostraremos consejos y trucos sobre cómo utilizar Doctrine para optimizar la velocidad de la base de datos y cómo diseñar una base de datos adecuadamente. También puede utilizar Doctrine para un proyecto existente (si cumple ciertas condiciones) y le mostraremos cómo hacerlo.

Esta serie de artículos se creó para ayudar a mis alumnos de formación y consultoría. Si necesita discutir o explicar ciertos temas con más detalle, puede enviarme un correo electrónico a jan@barasek.com. Dado que se trata de una tecnología relativamente exigente, todas las preguntas se tratarán como una consulta de pago.
