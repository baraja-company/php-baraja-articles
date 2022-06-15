Formulario plurianual
=====================

> id: '2abacddd-8a6b-4d25-a387-603fc7abc333'
> slug:
> 	cs: vicekrokovy-formular
> 	es: formulario-plurianual
> 
> publicationDate: '2019-09-16 09:30:19'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '69cb85c856478d588b96e9db9e695d97'

A veces tenemos que dividir el formulario en varias partes (páginas), procesarlas por separado y luego reunirlas en un solo resultado.

Este artículo describe métodos y patrones de diseño para hacerlo.

> **Nota:**
>
> El tema de dividir un formulario en varios pasos es muy complejo, sobre todo si se quiere hacer bien. Me he encontrado con muchos enfoques a lo largo de mi vida, de los que hablaré aquí. Algunos enfoques parecen atractivos, pero son ingenuos y sólo funcionan en casos concretos. Para cada enfoque, describo cuándo tiene sentido y qué enfoques no tienen sentido.

Diseño de una solución teórica
-------------------------

Por lo general, el objetivo es obtener los datos básicos del primer formulario en la primera página, validarlos, luego guardarlos "en algún lugar" y mostrar la siguiente página.

Cuando el usuario llega a la última página, hay que enviar todo el formulario y procesar las entradas.

En cada paso, es importante validar siempre cuidadosamente todos los datos y permitir que el usuario salte hacia atrás en las páginas a voluntad para que pueda volver a corregir los datos cuando encuentre un error. Además, si el formulario se debe renderizar de forma condicional en función de los datos ya obtenidos, se trata de un proceso muy exigente.

Aplicación de los formularios en sí
--------------------------------

Podemos implementar los formularios individuales nosotros mismos en HTML puro y luego manejar el procesamiento en PHP, o utilizar soluciones ya hechas como <a href="https://doc.nette.org/cs/3.0/forms">Formularios de Nette</a>.

> **Ejemplo de la vida:**
>
> Muy a menudo, los programadores principiantes me envían correos electrónicos y preguntan cuestiones aparentemente sencillas para las que hay una solución preparada. Por ejemplo, específicamente sobre el procesamiento de formularios en PHP.
>
> Siempre recomiendo saltarse el tratamiento manual por completo y utilizar una solución preparada en su lugar. En realidad, es muy complicado implementar correctamente, por ejemplo, la validación del correo electrónico introducido y que 2 contraseñas dentro de 2 campos coincidan, mientras que queremos redirigir al usuario de vuelta al formulario pre-rellenado según sus datos y con mensajes de error en caso de error.
>
> Porque la gente **no sabe que no sabe que no sabe** y por eso, en lugar de invertir 1 hora de tiempo en aprender una solución ya hecha para el 99,99% de los problemas, prefieren elegir su propia solución, a la que dedican decenas de horas de depuración, y todavía hay casos en los que los formularios no funcionan, arrojan errores, tienen vulnerabilidades de seguridad y no protegen los datos introducidos.

Así que el objetivo de este paso es implementar varias páginas en diferentes URLs que contendrán formularios en blanco.

Recomiendo implementar cada formulario independientemente de los otros (atómicamente) y manejar el paso de estado en una capa de aplicación diferente. La razón es que, en la práctica, cada formulario manejará la validación de datos de forma diferente, escribirá sus salidas de forma diferente, manejará los errores de forma diferente, y probablemente querremos ampliarlo o cambiarlo a lo largo del tiempo, por lo que no necesitamos conocer el contexto de todo el proceso y cambiar docenas de sitios para hacerlo.

Transferencia del Estado
---------------

Al procesar el primer formulario, queremos primero validar los datos recibidos y, si son correctos, redirigir al usuario al segundo paso. Es una buena idea manejar la redirección como una redirección HTTP, porque puede ocurrir fácilmente que los datos no sean válidos, en cuyo caso queremos devolver al usuario al primer formulario y no al siguiente paso.

Básicamente podemos almacenar los estados de 4 maneras:

**No se recomienda:**

- **No almacenar en ningún sitio** y transmitir completamente en la URL. Esto tiene la desventaja de que el usuario puede cambiar los datos ya enviados en la URL y así falsear la entrada. Por otra parte, podemos revelar información sensible, como las contraseñas, en la URL.
- **Añadir continuamente a <a href="/sessions">sesiones</a>**, es decir, insertar de forma incremental los datos recién adquiridos por clave en el campo. Esto tiene la desventaja de que si la aplicación comete un error, el usuario se queda atascado con las sesiones y no puede resolver el error de ninguna manera (aparte de borrar las cookies, lo que es extremadamente difícil para la mayoría de la gente), además con un formulario sin terminar se corre el riesgo de que los datos permanezcan pre-rellenados y puedan ser vistos por otra persona. Sin embargo, un caso mucho peor ocurre si la sesión tiene una validez muy corta (digamos 5 minutos) y el usuario pierde los datos del primer paso al rellenar el último... esto puede llegar a ser bastante molesto entonces.

**Recomendado:**

- Almacenamiento de la base de datos y paso del identificador. La primera vez que se envía el formulario, almacenamos todos los datos recogidos en una tabla de la base de datos y generamos un identificador aleatorio (digamos una cadena de 10 caracteres de longitud) para pasarlo entre páginas como parámetro. La ventaja de esto es que al procesar cualquier formulario, podemos escribir los datos recién recuperados y validados directamente en la tabla, y en caso de fallo, tenemos copias de seguridad físicas de los formularios analizados y podemos actuar sobre ellos. Por ejemplo, si un pedido está incompleto, podemos enviar un correo electrónico al usuario de que no lo ha completado y aumentar la posibilidad de una venta.
- El **Guardar en la cuenta actualmente conectada** funciona exactamente igual que el reenvío a través de la ID, excepto que en lugar de un identificador aleatorio (token) utilizamos una sesión a la ID del usuario actualmente conectado (si hay uno). La ventaja es que podemos mostrar los datos pre-rellenados al usuario arbitrariamente en el futuro.

Conclusión
-----

Ninguna de las soluciones anteriores es perfecta ni la única correcta. Yo mismo combino varios enfoques cuando trabajo en soluciones de varios pasos. Normalmente, por ejemplo, resuelvo una cesta de la compra como una tabla de la base de datos, a la que asigno los datos que ya he recogido y la vinculo a un usuario (si está conectado) o a una sesión (si no está conectado y aún no nos conocemos).
