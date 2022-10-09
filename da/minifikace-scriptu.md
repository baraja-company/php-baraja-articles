Automatisk minificering af PHP-script
=====================================

> id: f9225faf-f881-4f7d-8c0f-bab4cfea9cb9
> slug:
> 	cs: minifikace-scriptu
> 	da: automatisk-minificering-af-php-script
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '1a465671563e1a2d05dc17b93bb1c442'

Nogle gange har vi brug for at skrumpe et stort PHP-script og komprimere flere af dem i én fil. Dette er nyttigt, når vi opretter et bibliotek, som vi vil offentliggøre på nettet, og vi ikke ønsker, at nogen skal blande sig i det, eller når det er et nyttigt script, som vi ofte vil kopiere, og derfor ikke ønsker at overføre for mange data.

En mulig løsning er at minificere koden.

Jeg har forberedt et online værktøj til dette (indsæt blot koden, og du får straks den minificerede version tilbage).

Kernen i minifieren kan reduceres til dette minimum:

```php
$file = 'StaticClass.php';

// Dgx's PHP shrinker

// Kompatibilitet med PHP 4 og 5
if (!defined('T_DOC_COMMENT'))
  define ('T_DOC_COMMENT', -1);

if (!defined('T_ML_KOMMENTAR'))
  define ('T_ML_KOMMENTAR', -1);

// læse inddatafil
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

// skriv den krympede fil
file_put_contents('min_'.$file, $output);
```

Kernen er funktionen `token_get_all()`, som analyserer PHP-kode i individuelle "atomer" (tokens), som kan identificeres entydigt og derefter ignoreres efter behov.

For eksempel genererer den (til eksemplet brugte jeg metoden `Nette\Utils\Images`):

<img src="{$baseUrl}/images/nette-image-minify.png" alt="Minificeret kode">
