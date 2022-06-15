Calculadora en PHP: procesamiento de una expresión matemática como una cadena
=============================================================================

> id: e6798758-031d-4e7c-b24e-1f77cf61558d
> slug:
> 	cs: pokrocila-kalkulacka
> 	es: calculadora-en-php-procesamiento-de-una-expresion-matematica-como-una-cadena
> 
> publicationDate: '2020-02-16 17:07:38'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '63f5336bf2bbabe5312121b57e1ce34e'

Imagine que se enfrenta a la tarea de procesar un ejemplo matemático sencillo que un usuario introduce como cadena de texto, por ejemplo, en un cuadro de búsqueda. Normalmente, el usuario quiere realizar una operación numérica sencilla con números. Este artículo describe el proceso de reflexión y las instrucciones específicas sobre cómo hacerlo.

Aplicación ingenua
-------------------

Durante mucho tiempo me pregunté si una simple expresión matemática podría ser procesada por algún truco para hacer el código lo más corto posible... y después de muchos años realmente tengo la solución.

¡Considere la solución dada **sólo como un ejemplo**, porque es **extremadamente peligrosa** y un usuario deshonesto puede fácilmente subrayar la cadena, que por ejemplo borrará toda la aplicación o robará la base de datos!

```php
// Consulta del usuario
$query = '5 + 3 * 2';

// Procesar la expresión como código regular PHP
eval('$resultado = @(' . $query . ');');

// Listado de la variable con la solución de la expresión
echo $result; // imprime 11
```

El truco está en que la función <a href="/function-eval">eval()</a> ejecuta la cadena como si estuviera en el contexto del código PHP. Es una locura, pero funciona. La envoltura suprime los mensajes de error.

Tratamiento de entradas más complejas
--------------------------

Además del hecho de que **procesar expresiones a través de eval() es extremadamente peligroso**, tampoco proporciona una sintaxis lo suficientemente elocuente como para satisfacer a todo el mundo. Si el usuario comete una sola violación de la sintaxis, la expresión completa no podrá ser procesada.

Por lo tanto, la solución consiste en **comprender** y corregir la consulta del usuario según el aspecto formal primero (lo que se denomina normalizar a la forma canónica), y luego pasarla y procesarla más adelante.

He programado [QueryNormalizer](https://github.com/mathematicator-core/engine/blob/master/src/QueryNormalizer.php) exactamente para esta tarea en el pasado.

El procesamiento en sí es una tarea muy difícil, porque hay que entender correctamente los diferentes contextos. Por ejemplo, que los paréntesis denotan bloques anidados y deben ser evaluados recursivamente. Por ejemplo, la expresión `5+2^(1+3/2)` no puede resolverse directamente porque primero hay que resolver la fracción, sumarla al número del paréntesis, luego resolver la potencia entera y finalmente sumarla en la base.

Para poder cumplir este exigente requisito, la expresión ya no puede ser tratada como una cadena ordinaria y tenemos que **subir el nivel de abstracción**. En esencia, las matemáticas son un tipo de lenguaje que describe las relaciones entre las operaciones y los números, porque tenemos que tratar con las prioridades de los operadores, los diferentes significados, los contextos, la recursividad e incluso los tipos de datos. Ahí es donde entra el **proceso de tokenización de consultas**.

> He estado trabajando en el problema de la tokenización de las matemáticas desde 2015 y he escrito varios analizadores diferentes desde entonces.

El mejor de ellos, que actualmente impulsa el nuevo Mathematicator, es [disponible en código abierto en GitHub](https://github.com/mathematicator-core/tokenizer).

El objetivo de la tokenización es **analizar una cadena**, dividirla en grupos de cadenas más pequeñas de tipos conocidos y convertirlas en objetos (tipos de datos). La matriz de objetos convertida se convierte mediante **lógica inteligente en un árbol binario** que puede describir dependencias y recursividad. Es un proceso muy exigente porque hay cientos de escenarios posibles y los usuarios pueden ser muy creativos en sus consultas.

La principal ventaja de la matriz de tokens es que se puede pasar muy fácilmente a la siguiente capa, que por ejemplo [realiza el cálculo real](https://github.com/mathematicator-core/calculator), o [redibuja el árbol en LaTeX](https://github.com/mathematicator-core/tokenizer/blob/master/src/TokensToLatex.php).

El uso puede ser elegante como este:

```php
$tokenizer = new Tokenizer(/* algunas dependencias */);

// Convertir la fórmula matemática en un array de tokens:
$tokens = $tokenizer->tokenize('(5+3)*(2/(7+3))');

// Ahora puedes convertir los tokens a un formato más útil:
$objectTokens = $tokenizer->tokensToObject($tokens);

dump($objectTokens); // Devuelve tokens tipificados con metadatos

// Renderizar a LaTeX
echo $tokenizer->tokensToLatex($objectTokens);

// Renderizar al árbol de depuración (extremadamente rápido):
echo $tokenizer->renderTokensTree($objectTokens);
```

Ver procedimientos
-----------------

Un número importante de usuarios apreciará que **al calcular, el programa muestre el procedimiento** para mostrar cómo lo hizo. En realidad, esto también es útil para el programador, porque al menos puede averiguar fácilmente dónde hay un error en el cálculo y corregir el algoritmo en consecuencia. Cuando se combina todo esto con el aprendizaje automático basado en pruebas automatizadas, se obtiene algo sorprendente.

Mira cómo `QueryNormalizer` fue capaz de entender tu consulta, pasar los datos al tokenizador, éste renderizó la consulta en LaTeX de acuerdo con ella y luego pasó el árbol de objetos a la calculadora que devolvió el resultado global.

Příklad: [5+2^(1+3/2)](https://mathematicator.com/search/5%2B2%5E%281%2B3/2%29).

La representación del procedimiento se implementa mediante la calculadora que recorre el árbol de entrada y evalúa una regla a la vez según los tokens y reglas que contiene. Cuando se evalúa cualquier regla, pone la información del paso en un array. Ocasionalmente, un paso puede resultar incorrecto y tenemos que volver atrás y tomar un camino diferente en el cálculo, pero hay bastante magia detrás de eso, que permanecerá oculta por ahora y que puedes estudiar en la implementación.

Conclusión
-----

El procedimiento anterior describe cómo manejar elegantemente las expresiones matemáticas en las que tenemos números, operaciones y relaciones con ellos. Este enfoque no puede, por ejemplo, modificar expresiones o resolver ecuaciones, pero eso lo veremos la próxima vez.

*Si tienes otras ideas sobre cómo procesar las matemáticas de forma eficiente, estaré encantado de que me las cuentes.*
