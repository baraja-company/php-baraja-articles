Prueba de conocimientos básicos de Nette
========================================

> id: d137c177-503c-4e84-8709-0e65b0ce6060
> slug:
> 	cs: test-nette-zaklady
> 	es: prueba-de-conocimientos-basicos-de-nette
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '66fb122b33aa8ce41158345bb01769ab'

Umbral de éxito: 15 puntos

*Se obtiene 1 punto por cada pregunta contestada correctamente. Por cualquier pregunta mal contestada no obtienes nada. Si la respuesta es sólo parcial (y no sería posible programar la cosa basándose en ella), la pregunta cuenta como incorrecta (no es posible obtener medio punto). Si la solución contiene un error de seguridad, o una errata en el código, o un error tipográfico en el código, la respuesta se considera incorrecta porque no se ejecutaría.*

-----------

1 Explique la diferencia entre los bucles `for`, `while` y `foreach`. Para cada uno de ellos, dé un ejemplo concreto de su uso que muestre claramente su principal ventaja.


2. Tenemos una variable de la que no sabemos casi nada (sólo conocemos su nombre). ¿Cómo podemos ver su contenido? Por ejemplo, se llama `$data`.


3. Escribe los siguientes comandos para trabajar con el repositorio Git:
- Descargue los últimos cambios del servidor
- etiquetar el archivo `Statistic.php`.
- etiquetar todos los archivos del proyecto
- etiquetar todos los archivos en el directorio `cron`.
- confirmar los cambios con el mensaje "Mi confirmación".
- envío de la confirmación al servidor


4. Pongamos una cadena de texto en la variable. Pon un ejemplo de una función para calcular la suma de comprobación.


5. Escriba un fragmento de código que cree una acción `delete` en `Presenter` que acepte el ID del elemento como un número entero y elimine una fila de la tabla `question` según el ID especificado. Después de un borrado exitoso, imprimirá el mensaje "Pregunta borrada" y redirigirá a la acción `lista`.

Bajo la pregunta de un punto extra: Si el borrado falla por alguna razón, no arroja un error fatal, pero también informa al usuario sobre ello con un mensaje (mensaje flash).

6. Cuando creo un formulario Nette, se convierte en un componente. ¿Qué es un componente Nette?

7. Necesito crear un formulario simple de Nette para insertar un registro en una tabla de `preguntas` que contiene una lista de preguntas. La estructura de la tabla es:

| Columna de la columna de la derecha.
|-----------|----------------------------------|
| id | int(8), unsigned, auto increment |
| Pregunta varchar(255)
| is_active | tinyint(1), unsigned, valor por defecto: 1 |

Cree los campos de formulario adecuados para insertar una nueva fila en esta tabla. Después de insertar el registro, se debe disparar un FlashMessage informando de la inserción exitosa del registro + la redirección a la edición del registro (acción `edit`).

- Validación de que el campo del formulario ha sido rellenado
- Validación de que el texto de la pregunta encaja en el varchar según la estructura de la tabla
- Validación de que una pregunta con este texto ya no existe
- Defina otra tabla personalizada `group` para contener información sobre los grupos. Al crear una pregunta, se podrá determinar a qué grupo pertenece la pregunta. Tendrá que establecer una sesión entre las mesas (describa cómo se hace y cómo se establecerá).
- ¿Qué macro Latte utilizo para renderizar el formulario en la plantilla (renderización por defecto)?

8. Tenemos un formulario de edición en `Presenter` que se crea como un componente. Queremos pasar valores por defecto de lo que hay en la base de datos, es decir, necesitamos obtener los datos de la tabla de alguna manera conveniente.
- ¿Cómo va a proceder y qué elementos del código necesitamos?
- ¿Cómo pasará el identificador del elemento actualmente editado al formulario?
- ¿Cómo se establece el valor por defecto del elemento del formulario?
- ¿Cómo podemos verificar que un usuario está intentando editar un elemento que no existe en la base de datos, y cómo podemos informarle adecuadamente de ello?

9 Considere los siguientes datos recuperados de una base de datos (utilizando una base de datos Nette normal):

```php
$questions = $this->db->questions()->fetchAll();
```

- ¿Cómo podemos enumerar el texto de todas las preguntas en forma de lista con viñetas?
- ¿Cómo pasamos los datos de la tabla a la plantilla Latte?
- ¿Qué macros de Latte necesitaremos para enumerar los artículos? Dar una implementación específica de la lista de las columnas `id` y `nombre` en el formato:

	*1024: ¿Cómo estás?
	*1025: ¿Qué has comido hoy?

10. Enumere un ejemplo de al menos 3 campos de formulario diferentes que estén escritos en el formulario:

```php
$form->add(tady bude příklad);
```

y para cada uno explique para qué se utiliza y qué salida devuelve (tipo de datos + ejemplo).


11. Que se presente un formulario de Nette.
- ¿Cómo se envían todos los campos (nombres y valores)?
- Dé un ejemplo de salida de un campo llamado `pregunta`.
- Proporcionar una implementación concreta de código que recorra un array de valores y claves y devuelva una única cadena que contenga la lista total de claves y valores, de forma que podamos, por ejemplo, almacenar esta cadena en una base de datos o enviarla por correo electrónico (almacenar y enviar no es objeto de la asignación y no se evaluará).


12. Para cada condición, decida si el resultado es VERDADERO o FALSO:
- `1 > 0`
- `1 == 1`
- `1 == "1"`
- `1 === "1"`
- `1 == true`
- `1 === true`
- `1 === false`
- `1 == "1" && 1=== true`


13. ¿Qué tipos de datos conocemos en PHP?
- Dé al menos 5 ejemplos de nombres de tipos de datos
- Para cada ejemplo, enumere al menos 3 valores posibles que puedan almacenarse en el tipo de datos (si el tipo de datos no puede almacenar tantos valores posibles, escríbalo)
- Para cada tipo de datos, dé un ejemplo típico de su uso (lo que se almacena en él en la práctica)
- ¿Cuál es la diferencia de condición entre `==` (dos iguales) y `===` (tres iguales)?
- Explica la desventaja de usar `==` en condiciones y cómo resuelve específicamente `==` este problema (ejemplo en el que `==` puede fallar y `==` salva la situación)


14. Tengamos una tabla de coordinaciones (tabla de coordinaciones) que liste todas las coordinaciones entre 2 personas. Uno de ellos organiza la coordinación y el otro es un invitado. Escriba una selección en la base de datos que devuelva todas las filas con coordinaciones que me involucran (soy el organizador de la coordinación, o soy el invitado de la misma). La tabla tiene las columnas `id`, `id_user_organizer` (id de organizador), `id_user_quest` (id de invitado). Mi ID se almacena de la forma habitual en `Presenter`.


15. Grupo de preguntas sobre Latte:
- ¿Qué es el café con leche?
- ¿Cuál es la diferencia entre `variable`, `macro`, `filtro` y `n:attribute`? ¿Qué se utiliza y dónde?
- ¿Cómo puedo crear una referencia `DashboardPresenter` a una acción `default`?
- Al listar una tabla de preguntas, ¿cómo puedo generar una referencia a una edición específica (`QuestionPresenter`, acción `edit`) de una pregunta para pasar el ID de la pregunta actualmente listada? Escriba el código específico de Latte.

Escrito simbólicamente (ejemplo en PHP, traducido a Latte):

```php
foreach ($questions as $question) {
   echo $question->id; // ID de la pregunta
   echo $question->question; // texto de la pregunta
}
```

- ¿Cómo escribir un espacio HTML fijo?


16. ¿Para qué se utilizan los servicios en Nette?
- ¿Cómo se instala?
- ¿Qué tiene que hacer un servicio para ser utilizado?
- ¿Cómo se carga un servicio en Presenter?
- Por ejemplo, considere el servicio `StatisticManager`, que tiene un método público `getStatistics()` que no acepta parámetros. ¿Cómo puedo cargar este servicio en Presenter y llamar al método público `getStatistics()` en la acción por defecto y pasar el resultado a la plantilla?
- ¿Cuál es la diferencia entre `objeto`, `clase` y `servicio`?
- ¿Qué es un "modelo", una "entidad" y un "objeto de valor"?
- Todos los servicios tienen un gestor principal que los conoce y puede ser recogido en ese gestor. ¿Cómo se llama este gestor? ¿Cómo puedo registrar un nuevo servicio en él?


17. Neón
- ¿Qué son los archivos de neón?
- ¿Qué tipos hay y por qué se clasifican?
- ¿Qué contienen los archivos Neon? ¿Qué datos se almacenan en ellos?
- Dé un ejemplo de cómo registrar el siguiente campo (la receta está en PHP, habrá que traducirla) para que pueda ser utilizado como parámetro:

```php
$imageGenerator = [
   "puntos" => [
      480: [910, 30, 1845, 1150],
      600: [875, 95, 1710, 910],
      768: [975, 130, 1743, 660]
   ]
];
```

- Poner un ejemplo de registro de un servicio en Neon, al que le pasamos el parámetro `imageGenerator` que registramos en la tarea anterior, para que el servicio lo reciba en el constructor y pueda utilizarlo en el servicio (en el sentido de la configuración). Para el servicio, proporcione una implementación de ejemplo del constructor para que el primer parámetro de entrada sea tratado como el tipo de datos para el array.


18. Objetos en general
- ¿Qué son los `métodos`, las `propiedades` y las `constantes`? ¿Cuál es la diferencia entre ellos?
- Tanto los métodos como las propiedades tienen 3 estados básicos de accesibilidad (`público`, `privado`, `protegido`), explica la diferencia y un ejemplo concreto de uso y quién puede ver qué y cuándo.
- Tengo una clase `course` en la que hay una propiedad privada `currentCourse` en la que se almacena el curso actual. ¿Cómo hacer que la propiedad sea de sólo lectura y no se pueda escribir desde el exterior?


19. Cuando creo tablas en una base de datos que están lógicamente relacionadas (por ejemplo, una tabla para un usuario y luego una tabla para sus artículos), necesito tratar que los datos se vinculen correctamente.
- ¿Qué garantiza que los datos de las tablas estén correctamente enlazados en la base de datos?
- ¿Qué es la integridad referencial y cuál es su función en la base de datos?
- ¿Qué tipo de sesiones tenemos? ¿Cuál es la finalidad de cada tipo?
- ¿Qué parámetros establecemos para que las sesiones manejen la eliminación o modificación de datos de diferentes maneras? Poner 3 ejemplos y usos concretos + descripción de su funcionamiento.


20. ¿Cuál es el propósito de las fábricas (patrón de diseño `OOP`)?
- Pon un ejemplo de uso.
- ¿Los servicios de Nette son fábricas?
- ¿Cuál es el objetivo de la inyección de dependencia?
- ¿Cuál es la diferencia entre `DI` y `DIC`?
