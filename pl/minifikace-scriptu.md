Automatyczna minifikacja skryptu PHP
====================================

> id: f9225faf-f881-4f7d-8c0f-bab4cfea9cb9
> slug:
> 	cs: minifikace-scriptu
> 	pl: automatyczna-minifikacja-skryptu-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '1a465671563e1a2d05dc17b93bb1c442'

Czasami musimy zmniejszyć duży skrypt PHP i skompresować kilka z nich do jednego pliku. Jest to przydatne, gdy tworzymy bibliotekę, którą będziemy publikować w sieci i nie chcemy, aby ktokolwiek w nią ingerował, lub gdy jest to użyteczny skrypt, który będziemy często kopiować i nie chcemy przesyłać zbyt wielu danych.

Możliwym rozwiązaniem jest zminiaturyzowanie kodu.

Przygotowałem do tego celu narzędzie online (wystarczy wkleić kod, a natychmiast otrzymasz zminiaturyzowaną wersję).

Rdzeń minifikatora można zredukować do tego minimum:

```php
$file = 'StaticClass.php';

// Obkurczacz PZP firmy Dgx

// Zgodność z PHP 4 i 5
if (!defined('T_DOC_COMMENT'))
  define ('T_DOC_COMMENT', -1);

if (!defined('T_ML_COMMENT'))
  define ('T_ML_COMMENT', -1);

// odczyt pliku wejściowego
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

// zapisz zmniejszony plik
file_put_contents('min_'.$file, $output);
```

Rdzeniem jest funkcja `token_get_all()`, która przetwarza kod PHP na pojedyncze "atomy" (tokeny), które mogą być jednoznacznie identyfikowane, a następnie ignorowane w razie potrzeby.

Na przykład generuje on (w przykładzie użyłem metody `Nette\Utils\Images`):

<img src="{$baseUrl}/images/nette-image-minify.png" alt="Zminimalizowany kod">.
