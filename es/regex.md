Expresiones regulares en PHP
============================

> id: '32142161-6ace-4cfd-b472-b48031219e9b'
> slug:
> 	cs: regex
> 	es: expresiones-regulares-en-php
> 
> perex:
> 	- 'Regulární výrazy a jejich kompletní vysvětlení. Hledání podřetězce, pokročilé nahrazování a generování řetězců.'
> 	- 'Expresiones regulares y su explicación completa. Búsqueda de subcadenas, sustitución avanzada y generación de cadenas.'
> 
> publicationDate: '2020-03-08 13:37:38'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '392c8f14e6943dd8f345a9a134e0de92'

Las expresiones regulares son herramientas que permiten buscar, validar, comparar, dividir, contraer y reemplazar fácilmente cadenas de acuerdo con una máscara (patrón). Es una herramienta muy potente y elegante para la manipulación avanzada de cadenas.

Máscara
-----

En primer lugar, tenemos que crear la expresión regular que vamos a ejecutar. Se introduce como una cadena de texto, que tiene un montón de reglas y opciones de configuración (es una técnica muy compleja).

Para empezar, es importante tener en cuenta que la expresión regular se evalúa secuencialmente de izquierda a derecha, y si hay varias formas de interpretar la cadena, siempre se utiliza la mayor coincidencia posible (se comporta de forma **hambrienta**, tratando de procesar el mayor número de caracteres posible).

El comportamiento y la estrategia de procesamiento de una expresión regular pueden ser influenciados por interruptores, de los cuales hay muchos.

Simple verificación de que la cadena es un correo electrónico válido
-----------------------------------------------

¿Cómo podemos comprobar simplemente que la cadena `jan@barasek.com` es una dirección de correo electrónico válida sin tener que dividirla en partes complejas o recorrerla carácter por carácter?

Las expresiones regulares dan la respuesta (la expresión anterior está muy simplificada a efectos del ejemplo, y una implementación real de la validación de direcciones de correo electrónico debería ser un poco más complicada):

```php
$mail = 'jan@barasek.com';
$regex = '/^.+@.+\\N.(es|en|com)$/';

if (preg_match($regex, $mail)) {
   echo 'El correo electrónico es válido';
} else {
   echo 'El correo electrónico no es válido';
}
```

Examinemos la expresión `/^.+@.+\.(es|en|com)$/` con un poco más de detalle:

En primer lugar, tenemos que envolver toda la expresión en un par de caracteres `/` (al principio y al final) que indiquen dónde empieza y termina la expresión. El `/` al final de la expresión va seguido de cualquier modificador (configuración del modo de procesamiento de la expresión).

Al procesar una expresión, se procede desde el lado izquierdo carácter por carácter. Cada uno de ellos tiene su propio significado, que se recoge en la siguiente tabla:

| Carácter, significado, descripción, ejemplo, etc.
|------|-----------------|-------|-------------
| `^` | Inicio de la cadena | Fuerza que la cadena debe comenzar en este punto. | Fuerza que la cadena debe comenzar con la secuencia `+420` (útil para la validación de números, por ejemplo): `/^+420/`. |
| `$` | Fin de cadena o de línea | Obliga a que la cadena o la línea acabe aquí. El final de línea se escapa con `\z`. <a href="https://phpfashion.com/vite-co-znamena-v-regularnim-vyrazu">Explicación detallada</a>. | El nombre del archivo debe ser un archivo de texto (que termine con un punto y luego la cadena "txt"): `/\.txt$/`. |
| `.` | Cualquier carácter | Atrapa exactamente cualquier carácter. | Verifica que la cadena contiene exactamente uno de cualquier carácter: `/^.$/`. |
| Detecta un número de teléfono que no contenga espacios y tenga 9 dígitos: `/^(\+420)?\Nd{9}$/`.
| `\s` | Espacio en blanco | Atrapa espacios, guiones y tabuladores. | Permite espacios entre los dígitos de un número de teléfono en los dígitos triples: `/^(\d{3}\s?){3}$/`.
| `+` | Múltiples caracteres, pero al menos uno | Repite la subexpresión anterior e intenta atrapar el máximo posible. La subexpresión debe repetirse al menos una vez. | Atrapa todos los dígitos posibles, pero al menos uno: `/\d+/`. |
| `*` | Múltiples caracteres, puede ser ninguno | Funciona igual que `+`, pero permite atrapar una cadena vacía (el valor no tiene que estar presente). | Atrapa tantos dígitos como sea posible, si no existe ninguno, atrapa una cadena vacía: `/\d*/`.
| (` `)` | Paréntesis | Indica una subexpresión. Esto se puede utilizar para encerrar varias etiquetas diferentes y luego requerir, por ejemplo, la repetición sobre ellas, o para atrapar su contenido en una variable. | Dividamos el código postal en 2 partes según el espacio, que es opcional y puede haber incluso más de uno: `/^(\d{3})\s*(\d{2})$/` |
| `\|` | O | Contiene una subexpresión, u otra subexpresión. | Número que empieza por `+420` o `+421`: `/^+(420\|421)\s*\d+$/`. |
| `\.` | Escapar | Si queremos atrapar un carácter en una expresión que de otra manera tiene un significado especial, necesitamos escaparlo de la misma manera que, por ejemplo, las cadenas en PHP. | Atrapa un par de números separados por un punto (si no escapáramos el punto, se entendería como "cualquier carácter"): `/\d+.\d+/`.

Sólo para completar, daré la forma completa de la regla de validación para el correo electrónico tal como la implementa Nette:

```php
/**
 * Encuentra si una cadena es una dirección de correo electrónico válida.
 */
public static function isEmail(string $value): bool
{
   $atom = "[-a-z0-9!#$%&'*+/=?^_`{|}~]"; // RFC 5322 caracteres no entrecomillados en la parte local
   $alpha = "a-z\x80-\xFF"; // superconjunto de IDN
   return (bool) preg_match("(^
      (\"([ !#-[\\]-~]*|\\\\[ -~])+\"|$atom+(\\.$atom+)*)  # quoted or unquoted
      @
      ([0-9$alpha]([-0-9$alpha]{0,61}[0-9$alpha])?\\.)+    # domain - RFC 1034
      [$alpha]([-0-9$alpha]{0,17}[$alpha])?                # top domain
   \\z)ix", $value);
}
```

`preg_match()` - validación por patrón
------------------------------------

La función básica para la validación del formato y el análisis sintáctico es `preg_match()`, tiene 2 parámetros obligatorios y el tercero se puede utilizar para especificar el campo de salida.

Ejemplo:

```php
$psc = '272 01'; // Kladno

if (preg_match('/^(\d{3})\s*(\d{2})$/', $psc, $parser)) {
   echo 'El código postal es válido [' . $parser[1] . ','. $parser[2] . ']';
} else {
   echo 'El código postal no es válido';
}
```

El código devolverá: `Código válido [272, 01]`.

Obsérvese el paréntesis simple, que hemos utilizado para dividir la expresión en varias partes más pequeñas. Esto permite obtener las subexpresiones individuales como entradas de la matriz. La función completa devuelve entonces `true` o `false` dependiendo de si la cadena fue capturada con éxito.

Sin embargo, a veces, navegar por el orden numérico de los paréntesis es muy difícil, ya que el número puede cambiar, o simplemente puede haber demasiados. En este caso, es posible nombrar los corchetes individualmente y luego acceder a las llaves utilizando sus nombres.

Por ejemplo:

```php
$phone = '777 123 456';

preg_match('/^(?<operador>\d{3})\s*(?<número>[0-9 ]+)$/', $phone, $parser);

echo $parser['operador']; // devuelve 777
```

`preg_replace()` - reemplazo por patrón
----------------------------------------

También es posible reemplazar cadenas mediante regex, lo que resulta especialmente útil para diversas correcciones de formato posteriores al usuario.

Supongamos que queremos almacenar un número de teléfono introducido por el usuario en un número entero, ya que esto es requerido por una biblioteca de terceros, pero los usuarios pueden introducirlo en algunos formatos bastante salvajes.

En ese caso, me adhiero al dictado:

> "Sé generoso en lo que recibes y estricto en lo que envías".

Por eso ajustamos automáticamente el formato. En primer lugar, utilizamos el análisis sintáctico para dividir la cadena en sus partes individuales y, a continuación, la doblamos según los números de corchetes:

```php
function formatPhoneNumber(string $phoneNumber): int
{
   return (int) preg_replace(
      '/^(\+\d{3})\s*(\d{3})\s*(\d{3})\s*(\d{3})$/',
      '$2$3$4',
      $phoneNumber
   );
}

echo formatPhoneNumber('+420 777 123 456');
```

Construir una cadena según una expresión regular
----------------------------------------

Las remezclas también tienen mucho sentido cuando se generan nuevas cadenas según un patrón complejo.

PHP puro no tiene soporte para esto, pero podemos descargar una biblioteca de terceros <a href="https://github.com/icomefromthenet/ReverseRegex">ReverseRegex</a> que puede hacer esto.

Por ejemplo, podemos querer generar un conjunto de contraseñas basadas en el regex `[a-z]{10}` y nada nos lo impedirá:

```txt
jmceohykoa
aclohnotga
jqegzuklcv
ixdbpbgpkl
kcyrxqqfyw
jcxsjrtrqb
kvaczmawlz
itwrowxfxh
auinmymonl
dujyzuhoag
vaygybwkfm
```

El uso es el siguiente:

```php
use ReverseRegex\Lexer;
use ReverseRegex\Random\SimpleRandom;
use ReverseRegex\Parser;
use ReverseRegex\Generator\Scope;

require 'vendor/autoload.php';

$lexer = new  Lexer('[a-z]{10}');
$gen   = new SimpleRandom(10007);
$result = '';

$parser = new Parser($lexer, new Scope(), new Scope());
$parser->parse()->getResult()->generate($result, $gen);

echo $result;
```

Yo genero mis ejemplos matemáticos en Nette en Presenter, por ejemplo, de esta manera y es posible con verdadera facilidad:

```php
public function actionRegex(): void
{
   $lexer = new Lexer('\d{1,3}[\+\-\*\/]');
   $parser = new Parser($lexer, new Scope(), new Scope());
   for ($i = 0; $i <= 10; $i++) {
      $result = '';
      $gen = new SimpleRandom($i);
      $parser->parse()->getResult()->generate($result, $gen);
      dump($result);
   }
   $this->terminate();
}
```

Lo importante para la biblioteca es que sigue generando la misma salida para la misma entrada (aunque pueda parecer que hay muchas cadenas posibles de coincidir para cada expresión regular). Si queremos cambiar la expresión generada de forma aleatoria, también tenemos que cambiar la "semilla" con la que se genera la cadena de salida. Para ello, puede ser útil recorrer el intervalo de semillas o la función `rand(1, 1e6)`.

Captura y procesamiento de errores
-----------------------------

En PHP, la captura de errores en las regexes es bastante infernal, pero todavía hay una solución.

Esto se explica en detalle en el artículo <a href="https://phpfashion.com/zradne-regularni-vyrazy-v-php">Expresiones regulares alcanzables en PHP</a> de David Grudel.
