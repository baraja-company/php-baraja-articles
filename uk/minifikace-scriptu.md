Автоматична мінімізація PHP скрипта
===================================

> id: f9225faf-f881-4f7d-8c0f-bab4cfea9cb9
> slug:
> 	cs: minifikace-scriptu
> 	uk: avtomaticna-minimizaciya-php-skripta
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '1a465671563e1a2d05dc17b93bb1c442'

Іноді нам потрібно зменшити великий PHP-скрипт і стиснути кілька з них в один файл. Це корисно, коли ми створюємо бібліотеку, яку опублікуємо в Інтернеті і не хочемо, щоб хтось втручався в неї, або це корисний скрипт, який ми будемо часто копіювати і тому не хочемо передавати занадто багато даних.

Можливим рішенням є мінімізація коду.

Я підготував для цього онлайн-інструмент (просто вставте код і ви одразу отримаєте мініатюрну версію).

До цього мінімуму можна скоротити серцевину мініфікатора:

```php
$file = 'StaticClass.php';

// Dgx's PHP shrinker

// Сумісність з PHP 4 та 5
if (!defined('T_DOC_COMMENT'))
  define ('T_DOC_COMMENT', -1);

if (!defined('T_ML_COMMENT'))
  define ('T_ML_COMMENT', -1);

// прочитати вхідний файл
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

// записати стиснутий файл
file_put_contents('хв'.$file, $output);
```

Ядром є функція `token_get_all()`, яка розбирає PHP-код на окремі "атоми" (токени), які можна однозначно ідентифікувати, а потім ігнорувати за необхідності.

Наприклад, генерує (для прикладу я використовував метод `Nette\Utils\Images`):

<img src="{$baseUrl}/images/nette-image-minify.png" alt="Minified code">
