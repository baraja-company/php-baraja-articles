Minificazione automatica dello script PHP
=========================================

> id: f9225faf-f881-4f7d-8c0f-bab4cfea9cb9
> slug:
> 	cs: minifikace-scriptu
> 	it: minificazione-automatica-dello-script-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '1a465671563e1a2d05dc17b93bb1c442'

A volte abbiamo bisogno di rimpicciolire un grande script PHP e comprimerne diversi in un unico file. Questo è utile quando stiamo creando una libreria che pubblicheremo sul web e non vogliamo che qualcuno interferisca con essa, o è uno script utile che copieremo spesso e quindi non vogliamo trasferire troppi dati.

Una possibile soluzione è quella di minificare il codice.

Ho preparato uno strumento online per questo (basta incollare il codice e si ottiene immediatamente la versione minificata).

Il nucleo del minificatore può essere ridotto a questo minimo:

```php
$file = 'StaticClass.php';

// Il restringimento PHP di Dgx

// compatibilità con PHP 4 e 5
if (!defined('T_DOC_COMMENT'))
  define ('T_DOC_COMMENT', -1);

if (!defined('T_ML_COMMENT'))
  define ('T_ML_COMMENT', -1);

// leggere il file di input
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

// scrivere il file rimpicciolito
file_put_contents('min_'.$file, $output);
```

Il nucleo è la funzione `token_get_all()`, che analizza il codice PHP in singoli "atomi" (token) che possono essere identificati in modo univoco e poi ignorati come necessario.

Per esempio, genera (per l'esempio ho usato il metodo `Nette\Utils\Images`):

<img src="{$baseUrl}/images/nette-image-minify.png" alt="Codice minimizzato">
