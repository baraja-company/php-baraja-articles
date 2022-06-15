Minificación automática del script PHP
======================================

> id: f9225faf-f881-4f7d-8c0f-bab4cfea9cb9
> slug:
> 	cs: minifikace-scriptu
> 	es: minificacion-automatica-del-script-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '1a465671563e1a2d05dc17b93bb1c442'

A veces necesitamos reducir un script PHP grande y comprimir varios de ellos en un solo archivo. Esto es útil cuando estamos creando una biblioteca que publicaremos en la web y no queremos que nadie interfiera en ella, o es un script útil que copiaremos a menudo y por lo tanto no queremos transferir demasiados datos.

Una posible solución es minificar el código.

He preparado una herramienta en línea para ello (sólo tienes que pegar el código y obtendrás inmediatamente una versión minificada del mismo).

El núcleo del minificador puede reducirse a este mínimo:

```php
$file = 'StaticClass.php';

// Dgx's PHP shrinker

// Compatibilidad con PHP 4 y 5
if (!defined('T_DOC_COMMENTO'))
  define ('T_DOC_COMMENTO', -1);

if (!defined('T_ML_COMENTARIO'))
  define ('T_ML_COMENTARIO', -1);

// leer el archivo de entrada
$input = file_get_contents($file);

$space = $output = '';
$set = '!"#$&\'()*+,-./:;<=>?@[\]^`{|}';
$set = array_flip(preg_split('//',$set));

foreach (token_get_all($input) as $token)  {
  if (!is_array($token))
    $token = array(0, $token);

  switch ($token[0]) {
    case T_COMMENT:
    case T_ML_COMMENT:
    case T_DOC_COMMENT:
    case T_WHITESPACE:
      $space = '';
      break;

    default:
      if (isset($set[substr($output, -1)]) ||
          isset($set[$token[1]{0}])) $space = '';
      $output .= $space . $token[1];
      $space = '';
  }
}

// escribir el archivo reducido
file_put_contents('min_'.$file, $output);
```

El núcleo es la función `token_get_all()`, que analiza el código PHP en "átomos" individuales (tokens) que pueden ser identificados de forma única y luego ignorados según sea necesario.

Por ejemplo, genera (para el ejemplo he utilizado el método `Nette\Utils\Images`):

<img src="{$baseUrl}/images/nette-image-minify.png" alt="Código minificado">
