Automatická minifikace PHP scriptu
==================================

> id: f9225faf-f881-4f7d-8c0f-bab4cfea9cb9
> slug:
> 	cs: minifikace-scriptu
> 
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "95374429-e651-46bd-9149-15aa716f8207"

Občas potřebujeme zmenšit velký PHP script a stlačit jich několik do jednoho souboru. To se hodí třeba v případě, kdy vytváříme knihovnu, kterou budeme publikovat na internet a nechceme, aby nám do ní někdo zasahoval, nebo jde o užitečný script, který budeme často kopírovat a nechceme proto přenášet zbytečně mnoho dat.

Možným řešeným je minifikace kódu.

Připravil jsem pro to online nástroj (stačí vložit kód a ihned dostanete zpět jeho minifikovanou verzi).

Jádro minifikátoru lze po zestručnění zmenšit na toto minimum:

```php
$file = 'StaticClass.php';

// Dgx's PHP shrinker

// PHP 4 & 5 compatibility
if (!defined('T_DOC_COMMENT'))
  define ('T_DOC_COMMENT', -1);

if (!defined('T_ML_COMMENT'))
  define ('T_ML_COMMENT', -1);

// read input file
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
      $space = ' ';
      break;

    default:
      if (isset($set[substr($output, -1)]) ||
          isset($set[$token[1]{0}])) $space = '';
      $output .= $space . $token[1];
      $space = '';
  }
}

// write shrinked file
file_put_contents('min_'.$file, $output);
```


Jádro tvoří funkce `token_get_all()`, která naparsuje PHP kód na jednotlivé "atomy" (tokeny), které lze už jednoznačně poznat a podle potřeby následně ignorovat.

Vygeneruje například (pro ukázku jsem použil metodu `Nette\Utils\Images`):

<img src="{$baseUrl}/images/nette-image-minify.png" alt="Minifikovaný kód">
