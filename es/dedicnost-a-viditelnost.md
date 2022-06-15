Herencia y visibilidad en el EPI
================================

> id: '71896a55-dcfd-4bb6-9f53-64f22b1514eb'
> slug:
> 	cs: dedicnost-a-viditelnost
> 	es: herencia-y-visibilidad-en-el-epi
> 
> publicationDate: '2020-02-16 22:17:05'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '4584f3dcfdcfdc506d34a37c36fe6dd1'

Una de las propiedades fundamentales de la Programación Orientada a Objetos es la **herencia** y la <a href="/encapsulación">encapsulación</a>. Con estas características, podrá construir fácilmente una lógica de aplicación compleja manteniendo una buena legibilidad de la implementación.

El principio de la herencia
-------------------

La herencia expresa que la implementación de una clase se basa en otra. En la terminología OOP, hablamos de **descendiente** (la clase que hereda) y **ancestro** (la clase que heredamos).

En general, la herencia funciona haciendo que el descendiente obtenga todas las características del ancestro, ya sea adoptándolas exactamente como las tenía el ancestro, o modificándolas a su manera, o anulándolas completamente y utilizando su propia implementación.

El uso de este enfoque es muy amplio, y la herencia es utilizada por un número de <a href="/design-patterns">patrones de diseño</a>.

Uso real de la herencia - presentadores en una aplicación
--------------------

La herencia es muy adecuada para diseñar los llamados **presentadores**, que es un tipo especial de clase que representa la lógica de enlace en el patrón de diseño **MVC**.

Por ejemplo, tengamos un trío de páginas `Homepage`, `Contact` y `Login`.

A medida que se implementa cada página, gran parte de la lógica se repetirá (por ejemplo, aceptar una solicitud, construir la URL, renderizar la plantilla y enviar el HTML resultante). Por lo tanto, es conveniente implementar un único ancestro con esta lógica y sólo utilizarlo en los descendientes.

Empezamos por definir primero el ancestro (el nombre de la clase no importa, estoy usando una convención del marco Nette):

```php
abstract class BasePresenter
{
   public function link(string $route, array $params = []): string
   {
      // implementación del método para construir la URL
   }

   public function renderTemplate(string $path, array $params = []): string
   {
      // lógica de renderización de la plantilla
   }
}
```

Al definir la clase, utilicé la nueva palabra clave `abstract`, que dice que la clase `BasePresenter` es abstracta. Esto significa que no podemos crear una instancia de la misma, y sólo necesitamos usarla para que otra clase la herede y la implemente. La abstracción tiene otras ventajas útiles, que discutiremos más adelante. Una clase no tiene que ser abstracta para ser heredable - es sólo una de las opciones posibles.

Ahora podemos implementar una segunda clase, por ejemplo `HomepagePresenter`:

```php
final class HomepagePresenter extends BasePresenter
{
   public function run(): void
   {
       // lógica de renderización
       $this->renderTemplate('página web', [
          'contactLink' => $this->link('Contacto:por defecto'),
       ]);
   }
}
```

Ahora tienes una clase `HomepagePresenter` que funciona. Tenga en cuenta que la clase es `final`, lo que significa que ya no puede ser heredada, garantizando que los métodos se utilizarán exactamente como los especificamos.

Cuando implementamos la clase, creamos un nuevo método `run()` que sólo puede manejar `HomepagePresenter`. Dentro del método llamamos al método `renderTemplate()` y a `link()`, que la clase no contiene. Sin embargo, esto no importa, porque la palabra clave `extends` nos dice de dónde deben heredarse los métodos, por lo que se utilizan.

Gracias a la herencia, pudimos lograr la reutilización del código porque, una vez escritos, los métodos pueden utilizarse en múltiples lugares.

Sobrescribir la implementación de un método específico
------------

Muy a menudo puede ser útil anular el comportamiento de un método particular durante la herencia. Por ejemplo, si quisiéramos cambiar el comportamiento del método `link()` del ejemplo anterior en `ContactPresenter`, quedaría así:

```php
final class ContactPresenter extends BasePresenter
{
   public function run(): void
   {
      // lógica de renderización
      echo $this->link('Página de inicio:por defecto', []);
   }

   public function link(string $route, array $params = []): string
   {
      return 'https://baraja.cz';
   }
}
```

Para anular la implementación, simplemente defina el método en el hijo de nuevo y sobrescriba el cuerpo del método. Lo importante es cumplir con la interfaz e implementar los mismos argumentos de entrada.

Visibilidad de la herencia
--------------------------

A veces nos gustaría ocultar algunos métodos durante la herencia y utilizarlos sólo internamente. Alternativamente, sólo permite su uso durante la herencia y no como interfaz pública.

Por lo tanto, en general, hay algunas reglas simples de visibilidad. Denotamos los métodos por `public`, `protected` o `private`, y las reglas de visibilidad son las siguientes:

- Cualquiera puede llamar a los métodos `públicos` desde cualquier lugar, es decir, al crear una instancia tanto de un ancestro como de un descendiente.
- Sólo un ancestro o descendiente puede llamar a los métodos `protegidos`, pero no pueden ser llamados desde la interfaz pública cuando se crea una instancia. Son métodos internos para lograr la herencia (una aplicación conveniente es por ejemplo al método `link()` del ejemplo anterior).
- Sólo la clase actual puede llamar a los métodos `privados`, independientemente de la herencia y la configuración de la interfaz pública.

Cambiar la visibilidad en tiempo de ejecución
----------------------------

En casos muy específicos, puede ser útil cambiar la visibilidad de un método en tiempo de ejecución y luego llamarlo. Esto es utilizado, por ejemplo, por varias bibliotecas de Doctrine.

Para cambiar la visibilidad, utilizamos entonces la clase nativa **ReflectionClass** implementada por el propio PHP.
