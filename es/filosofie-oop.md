Filosofía básica de la programación orientada a objetos
=======================================================

> id: c38214d9-8c92-4484-9c31-239d716f2545
> slug:
> 	cs: filosofie-oop
> 	es: filosofia-basica-de-la-programacion-orientada-a-objetos
> 
> publicationDate: '2020-01-02 22:21:56'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3841046b40a039413bacd045f275c68

La programación orientada a objetos es un paradigma, una visión de cómo programar. Pronto verás por ti mismo que la POO aporta una simplificación bastante fundamental a todos los problemas y dificultades comunes que se resuelven una y otra vez en la programación real.

La idea básica
-----------------

La idea básica de la programación orientada a objetos es dividir una gran aplicación (una tarea compleja) en muchas partes pequeñas que podamos resolver de forma elegante e independiente.

Por ejemplo, si programamos un sistema de reservas de billetes de avión, es un proyecto muy complejo que resuelve miles de casos. Si podemos descomponer toda la lógica compleja en muchas capas y partes, podemos entender fácilmente todo el sistema complejo y programar las subtareas individuales de forma independiente.

Ventajas prácticas de la POO y de la división del código en objetos
------------------------------------------------

Aparte del punto de vista académico, hay muchas razones prácticas para utilizar la POO:

- La aplicación creará una <a href="/encapsulación">encapsulación</a>, lo que significa que los datos se procesan siempre en un contexto local, que son pocos, por lo que no tenemos que pensar en toda la aplicación a la vez.
- Podemos dividir una aplicación en cientos de miles de pequeñas partes que pueden ser desarrolladas por diferentes personas en paralelo y simplemente organizar una interfaz común.
- Podemos ver la aplicación desde la perspectiva de diferentes capas (de forma abstracta en módulos completos, o localmente en funciones específicas que resuelven un algoritmo concreto).
- Al depurar la aplicación (debugging) podemos escalonar la aplicación, omitir o sustituir algunas partes.
- Podemos aplicar mejor los **patrones de diseño** y así evitar errores y complejidades en el código antes de que se produzcan.

Personalmente, no me imagino a los equipos con más de una persona programando de forma diferente.

> **Nota:**
>
> El uso de objetos supone un poco más de memoria y sobrecarga de la CPU en el ordenador, por lo que se reducirá un poco el rendimiento de la aplicación. En un entorno real, sin embargo, esto no importa, porque al reprogramar la aplicación a objetos se pierde algo de rendimiento (normalmente unidades de porcentaje), pero se ahorra tiempo a los programadores (normalmente decenas o cientos de porcentaje). El tiempo humano es siempre mucho más caro (y muy limitado) que el tiempo del ordenador.
>
> No olvides también que la POO aporta una gran simplificación a toda la aplicación y permite completar grandes aplicaciones en un tiempo razonable. Un gran número de aplicaciones complejas sería casi imposible de programar sin objetos.

Relación con el mundo real
-------------------------

El objetivo básico de la POO en el diseño de software es simular al máximo las propiedades, comportamientos y principios del mundo real. Los objetos en POO representan entidades reales. Esta forma de pensar nos permite construir enormes sistemas complejos que se pueden entender bien, resolver problemas del mundo real internamente como se resolverían sin un ordenador, y los principios se pueden explicar a personas reales.

Por ejemplo, si estamos implementando una aplicación de gestión de contenidos, tiene sentido disponer toda la lógica interna en muchas entidades reales (artículo, autor, categoría) y construir las sesiones no según los ID de registro generados artificialmente (como se hace convencionalmente en las bases de datos), sino según las relaciones reales.

Ejemplo de aplicación concreta:

```php
class Article
{
    private Author $author;

    /** @var Categoría[] */
    private array $categories;

    private string $title;
}

class Author
{
    private string $name;
}

class Category
{
    private string $name;
}
```

Como podemos ver, la clase `Artículo` no sólo contiene parámetros técnicos como el ID del registro con el autor, sino que es un enlace real a la entidad Autor, que de nuevo tiene sus propias propiedades.

Para una explicación de la implementación y la sintaxis específicas, consulte el <a href="/uvod-do-oop">tutorial introductorio</a>.
