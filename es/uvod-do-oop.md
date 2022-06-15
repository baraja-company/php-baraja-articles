Introducción a la programación orientada a objetos en PHP
=========================================================

> id: '44a79461-dcfd-4dd0-b6fd-ba5db76db6de'
> slug:
> 	cs: uvod-do-oop
> 	es: introduccion-a-la-programacion-orientada-a-objetos-en-php
> 
> publicationDate: '2020-01-01 19:54:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '888db2cf35845892eebade9fdcc18871'

Bienvenido al primer artículo del curso online OOP en PHP. Para ver la lista completa de artículos, visite la página <a href="/oop">vistazo general</a>.

> **Notas de contenido:**
>
> El objetivo de esta serie es **explicar de la mejor manera posible la esencia** de la programación orientada a objetos para que no tengas que pasar cientos de horas experimentando con cosas que no tienen sentido. Todas las técnicas y tutoriales son de la práctica y se explican como hubiera querido leerlas yo mismo cuando aprendía a programar. Algunos **conceptos están muy simplificados** a expensas de la corrección del 100% de los hechos, porque creo que siempre es **mejor entender principalmente los principios** y tener al menos alguna idea de los temas que perderse en la programación y ver todo como un gran lío.
>
> Como pronto verás, programar en POO tiene enormes beneficios para tu código. Aportará un nuevo nivel de abstracción a su aplicación y le permitirá resolver problemas muy complejos que no serían posibles de otra manera.

Qué son los objetos
------------------

En el concepto básico de la programación, los objetos son tipos de datos especiales que, además de su propio valor, pueden definir el estado detallado, los métodos, el comportamiento y permiten realizar diversas operaciones, como en el mundo real.

Un objeto en programación es algo parecido a una "cosa" del mundo real que podemos nombrar (por ejemplo, con un nombre) y que tiene propiedades como una cosa del mundo real. Por ejemplo, un objeto puede ser un artículo que tiene algunos valores (título, contenido, autor, fecha de publicación, ...) y métodos (insertar nuevo título, leer contenido, calcular el número de días desde la publicación, ...). En este artículo, trataremos principalmente de cómo crear y trabajar con el objeto a un nivel básico.

Objetos y clases
---------------

Como ya hemos establecido, un **objeto es algo que existe**. La palabra "existe" es muy importante en esta fase, porque, como pronto veremos, todavía hay que ocuparse de la aplicación práctica en la programación.

Dado que en la programación estamos escribiendo código y un objeto es algo "vivo", a partir de este punto distinguiremos entre dos conceptos diferentes, para los que es importante entender el significado en detalle y distinguirlo estrictamente:

- Un **objeto** es algo "vivo" que existe ahora mismo (tiene una instancia), y puede ejecutar algo o reaccionar ante otros objetos.
- Una **clase** es un código escrito estáticamente que aún no ha sido evaluado y que permanece igual todo el tiempo.

Dado que al programar siempre escribimos el código como una cadena estática (texto), distinguiremos los programas en `clases` (definición estática de todo el objeto) y `objetos` (una instancia `viva` de la clase).

Ejemplo de definición de una clase:

```php
class Article
{
    public string $title;

    public ?string $autor = null;
}
```

> **Notas prácticas:**
>
> En una aplicación real, cada clase suele escribirse en un archivo PHP separado, cuyo nombre es el mismo que el de la clase.
>
> Así que para la clase `Artículo`, por ejemplo, creamos `src/Artículo.php`. Esta convención no es necesaria en PHP, pero ayuda a que la aplicación sea más legible para no tener mucho código junto en un solo lugar.
>
> Cada clase puede existir como máximo una vez en toda la aplicación (tener un nombre único), pero puede haber (casi) infinitas instancias (dependiendo de la capacidad de RAM). Además, una misma clase no puede dividirse en varios archivos y debe definirse en un único lugar de una vez. Si necesita crear varias clases con el mismo nombre, utilice **espacios de nombres**.
>
> También mostraremos más adelante que un código bien estructurado permitirá reutilizarlo en muchos proyectos.

Este código define una clase `Article` con dos propiedades (`title` y `author`). PHP ignora los comentarios y los utiliza sólo para la comprobación de tipos estáticos (normalmente leídos por PhpStorm).

Código:

```php
public string $title;
```

es la definición de `propiedades`, que es una propiedad de la clase `Artículo`. Podemos pensar que `Artículo` es un tipo de campo y `título` es su índice, en el que podemos escribir un valor. Cada propiedad debe tener definida una accesibilidad (`pública`, `protegida` o `privada`), más adelante explicaremos qué significa cada ajuste. Si no sabe qué accesibilidad elegir en un caso concreto, ponga "pública".

Instanciar una clase y crear un objeto
----------------------------------

El concepto de `instancia` es uno de los más difíciles de entender, por lo que es importante que leas las siguientes líneas con mucha atención y te recomiendo que pruebes todos los ejemplos.

Si nuestra aplicación ya tiene una clase definida en el código fuente, en realidad no se utiliza en ninguna parte y PHP no lo sabe. Esto se debe a que la POO introduce el `principio de encapsulación de datos`, que significa que una clase tiene un contexto interno (local) que sólo es válido dentro de la clase. Esto se debe a que en el desarrollo sin objetos, estábamos acostumbrados a evaluar siempre el código en su totalidad. Además, trate de pensar en una clase como una forma de función o programa llamado externamente.

Ahora podemos crear nuestra primera instancia, utilizando el nuevo operador `new` para hacerlo.

```php
$myArticle = new Article;
$myArticle->title = 'Mi primer artículo'; // escribe el valor

echo $myArticle->title; // lista "Mi primer artículo"
```

Tenga en cuenta que ponemos el objeto `Article` completo creado por el operador `new` en la variable `$myArticle`. Esto significa para nosotros que el objeto es realmente un `tipo de datos`, por lo que podemos moverlo a través de las variables y seguir trabajando con él.

El operador de flecha `->` se utiliza para manipular el objeto. Si tenemos una instancia de un objeto concreto (tenemos una variable con su contenido), podemos escribir fácilmente valores en las propiedades internas, o leerlos o cambiarlos de varias maneras utilizando métodos (lo veremos más adelante).

La **Instancia de un objeto es la creación de una copia local "viva" de la clase en memoria (variable).**

Creación de varias instancias de la misma clase al mismo tiempo
---------------------------------------------

Como ya hemos comentado, cada objeto tiene un contexto local que se aplica en su interior. Gracias a este principio, podemos crear muchas instancias independientes de la misma clase y manipularlas libremente. Incluso podemos crear un número dinámico de instancias y, por ejemplo, almacenarlas como elementos de un array. Las posibilidades son infinitas.

> **Atención:**
>
> Cuando se crea un número extremo de instancias (cientos o más), hay que tener en cuenta la huella de memoria, ya que PHP necesita mantener la información de todas las instancias en la RAM. En la práctica, sin embargo, no se produce el problema del desbordamiento de la memoria debido a muchas instancias.

Ejemplo:

```php
$firstArticle = new Article;
$firstArticle->title = 'Mi primer artículo';

$secondArticle = new Article;
$secondArticle->title = 'Sobre un perro y un gato';

echo $firstArticle->title;  // lista "Mi primer artículo"
echo $secondArticle->title; // escribe "Sobre un perro y un gato"
```

Tenga en cuenta que el listado de la propiedad `title` (`->title`) depende de la instancia de la que estemos leyendo.

Resumen
-------

Hemos mostrado cómo definir nuestra primera clase y crear una instancia de la misma (un objeto) en la que hemos escrito varios valores.

En la próxima entrega, explicaremos el concepto de <a href="/methods-and-passing-input">Constructor, métodos y paso de entrada</a>.
