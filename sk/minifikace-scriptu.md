Automatická minifikácia skriptu PHP
===================================

> id: f9225faf-f881-4f7d-8c0f-bab4cfea9cb9
> slug:
> 	cs: minifikace-scriptu
> 	sk: automaticka-minifikacia-skriptu-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '1a465671563e1a2d05dc17b93bb1c442'

Niekedy potrebujeme zmenšiť veľký skript PHP a skomprimovať niekoľko z nich do jedného súboru. To je užitočné, keď vytvárame knižnicu, ktorú budeme publikovať na internete a nechceme, aby do nej niekto zasahoval, alebo ide o užitočný skript, ktorý budeme často kopírovať a nechceme prenášať príliš veľa dát.

Možným riešením je minifikácia kódu.

Pripravil som na to online nástroj (stačí vložiť kód a ihneď sa vám vráti minifikovaná verzia).

Jadro minifirmy sa dá zredukovať na toto minimum:

```php
$file = 'StaticClass.php';

// PHP zmršťovač Dgx

// Kompatibilita s PHP 4 a 5
if (!defined('T_DOC_COMMENT'))
  define ('T_DOC_COMMENT', -1);

if (!defined('T_ML_COMMENT'))
  define ('T_ML_COMMENT', -1);

// čítanie vstupného súboru
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

// zapísať zmenšený súbor
file_put_contents('min_'.$file, $output);
```

Základom je funkcia `token_get_all()`, ktorá analyzuje kód PHP na jednotlivé "atómy" (tokeny), ktoré možno jednoznačne identifikovať a podľa potreby ignorovať.

Napríklad generuje (pre príklad som použil metódu `Nette\Utils\Images`):

<img src="{$baseUrl}/images/nette-image-minify.png" alt="Minifikovaný kód">
