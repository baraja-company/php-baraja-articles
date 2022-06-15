Patrones de diseño en PHP
=========================

> id: c22b3e54-bb66-4196-a998-f4b62b9308b2
> slug:
> 	cs: navrhove-vzory
> 	es: patrones-de-diseno-en-php
> 
> publicationDate: '2020-02-13 10:32:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '9ee32f0a3ce55bad52725f45fcd1c041'

Los patrones de diseño son formas de pensar en la programación.

Proporcionan una colección de consejos, prácticas preparadas, mejores prácticas y conocimientos sobre el desarrollo. Para cada paradigma de programación y tipo de tarea, hay ciertos patrones de diseño que son los más adecuados.

Ventajas: ¿por qué utilizar patrones de diseño?
---------------------------------------

En programación, la resolución de ciertos tipos de problemas es repetitiva, por lo que tiene sentido elegir un método para resolver estos problemas y seguir repitiendo ese método.

El principal beneficio surge especialmente durante el desarrollo en equipo, cuando todos saben cómo se desarrollará la aplicación (según qué patrón de diseño) y simplemente lo aplican. Así se eliminan las innecesarias decenas de horas de depuración de código extrañamente escrito y de intentar comprender los principios que el autor pretendía.

MVC - un ejemplo de uso de un patrón de diseño
--------------------------------------

Mi patrón de diseño favorito es `MVC` (de `Model View Controller`), que dice que la aplicación está dividida en 3 capas independientes que se llaman entre sí secuencialmente y se pasan datos entre sí.

Por ejemplo, al renderizar una página, podría parecer que primero decide qué tipo de página es (por ejemplo, un detalle de categoría), por lo que llama a `CategoryController` con el método `detail`.

Un ejemplo concreto (estoy simplificando mucho):

```php
class CategoryController
{
    public CategoryManager $categoryManager;

    public function actionDetail(string $id): void
    {
        $this->template->id = $id;
        $this->template->category = $this->categoryManager->getById($id);
    }
}
```

> **Nota:**
>
> Esto es sólo un código de ejemplo que explica el principio del patrón de diseño `MVC`.
>
> En una implementación real, tendríamos que averiguar además cómo, por ejemplo, obtener una instancia de `CategoryManager` y cómo pasarla a la propiedad. Normalmente, para este tipo de tareas se utiliza la "inyección de dependencias".

Antes de renderizar la página para el detalle de la categoría, el `CategoryController` es llamado primero para recibir la petición real (es decir, estamos renderizando el detalle de la categoría con un determinado ID, que es obtenido por el router, por ejemplo, en la URL), obtener los datos (consultando el `Model` correspondiente) y pasar los datos finales a la plantilla para su renderización.

La gran ventaja de este principio es que podemos escribir muchos modelos (lógica de la aplicación) que son independientes de la forma en que se presentan los datos (la plantilla), lo que da lugar a un código reutilizable. De hecho, si queremos utilizar el `CategoryManager` en otro proyecto, simplemente pasamos los datos a través del `Controller` de una manera específica, que se renderizará según la plantilla definida por el propio proyecto, mientras que la lógica de la aplicación sigue siendo la misma y a nadie le importa porque la capa de software ha cumplido con su interfaz y responsabilidades acordadas.

> **Nota práctica:**
>
> El patrón de diseño `MVC` es utilizado por la mayoría de los frameworks modernos como Nette, Symfony, Laravel y otros.
>
> También podemos encontrarnos con `MVC` en el desarrollo de aplicaciones móviles y otros tipos de software donde necesitamos obtener datos y renderizarlos en una plantilla según el tipo de página o vista.

Patrones de diseño importantes para el desarrollo web
---------------------------------------

Generalmente en programación hay muchos patrones de diseño que no son adecuados para el desarrollo web. Esta lista describe los patrones más importantes que yo mismo utilizo y con los que deberías estar familiarizado.

Puede encontrar <a href="/categories-design-patterns">una lista completa de todos los patrones de diseño</a>, ejemplos de su uso y explicaciones detalladas en una página aparte.

- **MVC** - El principio de dividir una aplicación en `Modelo` (lógica y datos de la aplicación), `Vista` (plantilla y vista de datos) y `Controlador` (que une el `Modelo` y la `Vista`).
- **Inyección de dependencias** - En lugar de crear instancias de clases, definimos los llamados servicios que hemos inyectado automáticamente. El código admite todas las dependencias, las partes individuales se pueden intercambiar y construimos la aplicación de forma dinámica.
- **Singleton (Singleton)** - Cada clase tiene una sola instancia (personalmente uso esto en combinación con `Inyección de dependencia`, donde cada servicio tiene una sola instancia que se pasa a través de la aplicación).
- Inicialización lenta (inicialización retardada)** - Creamos una instancia de un objeto cuando se necesita por primera vez.
- **Adaptador** - Si necesitamos proporcionar comunicación entre dos clases incompatibles, el `Adaptador` convierte los datos de un tipo al otro (típicamente convirtiendo tipos de datos nativos de PHP a tipos de datos de base de datos y viceversa).
- **Comando** - En lugar de ejecutar una tarea específica directamente (por ejemplo, enviar un correo electrónico), sólo recordamos que debía ocurrir. A continuación, otro o el mismo proceso recogerá la solicitud y la procesará. La principal ventaja es que podemos procesar la aplicación de forma perezosa (y por tanto muy rápida), reintentar acciones fallidas y simplemente almacenar los parámetros de la llamada. Al mismo tiempo, esto nos permitirá recibir peticiones de diferentes clientes, a través de APIs, etc. Las acciones enviadas también se pueden poner en cola, priorizar y posiblemente cancelar antes de la evaluación.
- **Estrategia** - Encapsula algún tipo de algoritmos u objetos a cambiar para que sean intercambiables para el cliente.

Hay muchos más patrones de diseño, estos son los más importantes que debes conocer.

Anti-patrón
------------

Algunas técnicas de desarrollo de programación se consideran "antipatrón", que es exactamente lo contrario de un patrón de diseño. Suele ser una técnica que produce un código extraño que no puede ser fácilmente depurado, mantenido, y que se comporta de forma "mágica".

Un ejemplo típico es el uso de <a href="/variable-global">variables globales</a>.
