Automatic minification of PHP script
====================================

> id: f9225faf-f881-4f7d-8c0f-bab4cfea9cb9
> slug:
> 	cs: minifikace-scriptu
> 	en: automatic-minification-of-php-script
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '1a465671563e1a2d05dc17b93bb1c442'

Sometimes we need to shrink a large PHP script and compress several of them into one file. This comes in handy when we are creating a library that we will publish to the web and don't want anyone to interfere with it, or it is a useful script that we will be copying often and don't want to transfer too much data.

A possible solution is to minify the code.

I have prepared an online tool for this (just paste the code and you will get back a minified version of it immediately).

The core of the minifier can be reduced to this minimum:

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
$set = '!"#$&()*+,-./:;<=>?@[\]^`{|}';
$set = array_flip(preg_split('//',$set));

foreach (token_get_all($input) as $token) {
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


The core is the `token_get_all()` function, which parses PHP code into individual "atoms" (tokens) that can be uniquely identified and then ignored as needed.

For example (I used the `Nette\Utils\Images` method for the example):

<img src="{$baseUrl}/images/nette-image-minify.png" alt="Minified code">
