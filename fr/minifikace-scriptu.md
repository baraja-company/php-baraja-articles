Minimisation automatique des scripts PHP
========================================

> id: f9225faf-f881-4f7d-8c0f-bab4cfea9cb9
> slug:
> 	cs: minifikace-scriptu
> 	fr: minimisation-automatique-des-scripts-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '1a465671563e1a2d05dc17b93bb1c442'

Parfois, nous avons besoin de réduire un grand script PHP et d'en compresser plusieurs en un seul fichier. C'est utile lorsque nous créons une bibliothèque que nous publierons sur le web et que nous ne voulons pas que quelqu'un interfère avec elle, ou qu'il s'agit d'un script utile que nous copierons souvent et que nous ne voulons donc pas transférer trop de données.

Une solution possible est de minifier le code.

J'ai préparé un outil en ligne pour cela (il suffit de coller le code et vous obtiendrez la version minifiée immédiatement).

Le noyau du hachoir peut être réduit à ce minimum :

```php
$file = 'StaticClass.php';

// Le rétrécisseur PHP de Dgx

// Compatibilité avec PHP 4 et 5
if (!defined('T_DOC_COMMENT'))
  define ('T_DOC_COMMENT', -1);

if (!defined('T_ML_COMMENT'))
  define ('T_ML_COMMENT', -1);

// lire le fichier d'entrée
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

// écrire le fichier rétréci
file_put_contents('min_'.$file, $output);
```

Le noyau est la fonction `token_get_all()`, qui analyse le code PHP en "atomes" individuels (tokens) qui peuvent être identifiés de manière unique et ensuite ignorés si nécessaire.

Par exemple, il génère (pour l'exemple, j'ai utilisé la méthode `Nette\Utils\Images`) :

<img src="{$baseUrl}/images/nette-image-minify.png" alt="Code minimisé">
