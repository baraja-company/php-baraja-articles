Automatisk minifiering av PHP-skript
====================================

> id: f9225faf-f881-4f7d-8c0f-bab4cfea9cb9
> slug:
> 	cs: minifikace-scriptu
> 	sv: automatisk-minifiering-av-php-skript
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '1a465671563e1a2d05dc17b93bb1c442'

Ibland behöver vi krympa ett stort PHP-skript och komprimera flera av dem till en fil. Detta är användbart när vi skapar ett bibliotek som vi kommer att publicera på webben och vi vill inte att någon ska störa det, eller när det är ett användbart skript som vi kommer att kopiera ofta och därför inte vill överföra för mycket data.

En möjlig lösning är att minska koden.

Jag har förberett ett online-verktyg för detta (klistra bara in koden så får du tillbaka den förminskade versionen omedelbart).

Minifierns kärna kan reduceras till detta minimum:

```php
$file = 'StaticClass.php';

// Dgx PHP-krympare

// Kompatibilitet med PHP 4 och 5
if (!defined('T_DOC_COMMENT'))
  define ('T_DOC_COMMENT', -1);

if (!defined('T_ML_KOMMENTAR'))
  define ('T_ML_KOMMENTAR', -1);

// läser inmatningsfilen
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

// skriver den krympta filen
file_put_contents('min_'.$file, $output);
```

Kärnan är funktionen `token_get_all()`, som analyserar PHP-kod i enskilda "atomer" (tokens) som kan identifieras unikt och sedan ignoreras vid behov.

Till exempel genererar den (för exemplet använde jag metoden `Nette\Utils\Images`):

<img src="{$baseUrl}/images/nette-image-minify.png" alt="Minifierad kod">
