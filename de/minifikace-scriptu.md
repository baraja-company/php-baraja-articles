Automatische Verkleinerung von PHP-Skripten
===========================================

> id: f9225faf-f881-4f7d-8c0f-bab4cfea9cb9
> slug:
> 	cs: minifikace-scriptu
> 	de: automatische-verkleinerung-von-php-skripten
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '1a465671563e1a2d05dc17b93bb1c442'

Manchmal müssen wir ein großes PHP-Skript verkleinern und mehrere davon in eine Datei komprimieren. Dies ist nützlich, wenn wir eine Bibliothek erstellen, die wir im Internet veröffentlichen wollen und nicht möchten, dass sich jemand daran zu schaffen macht, oder wenn es sich um ein nützliches Skript handelt, das wir oft kopieren werden und nicht zu viele Daten übertragen wollen.

Eine mögliche Lösung besteht darin, den Code zu verkleinern.

Ich habe dafür ein Online-Tool vorbereitet (fügen Sie einfach den Code ein, und Sie erhalten sofort eine verkleinerte Version davon zurück).

Der Kern des Minifiers kann auf dieses Minimum reduziert werden:

```php
$file = 'StaticClass.php';

// PHP-Shrinker von Dgx

// Kompatibilität mit PHP 4 und 5
if (!defined('T_DOC_COMMENT'))
  define ('T_DOC_COMMENT', -1);

if (!defined('T_ML_KOMMENTAR'))
  define ('T_ML_KOMMENTAR', -1);

// Eingabedatei lesen
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

// geschrumpfte Datei schreiben
file_put_contents('min_'.$file, $output);
```

Das Herzstück ist die Funktion `token_get_all()`, die PHP-Code in einzelne "Atome" (Token) zerlegt, die eindeutig identifiziert und dann bei Bedarf ignoriert werden können.

Zum Beispiel erzeugt es (für das Beispiel habe ich die Methode "Nette\Utils\Images" verwendet):

<img src="{$baseUrl}/images/nette-image-minify.png" alt="Minimierter Code">
