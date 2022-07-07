自动减化PHP脚本
=========

> id: f9225faf-f881-4f7d-8c0f-bab4cfea9cb9
> slug:
> 	cs: minifikace-scriptu
> 	zh: zi-dong-jian-huaphp-jiao-ben
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '1a465671563e1a2d05dc17b93bb1c442'

有时我们需要缩小一个大的PHP脚本，并将其中的几个脚本压缩成一个文件。当我们正在创建一个将发布到网络上的库，而我们不希望任何人干涉它时，或者它是一个我们将经常复制的有用的脚本，因此不希望传输太多的数据时，这很有用。

一个可能的解决方案是将代码最小化。

我为此准备了一个在线工具（只需粘贴代码，你将立即得到最小化的版本）。

缩减器的核心可以减少到这个最小值。

```php
$file = 'StaticClass.php';

// Dgx的PHP收缩器

// PHP4和5的兼容性
if (!defined('T_DOC_COMMENT'))
  define ('T_DOC_COMMENT', -1);

if (!defined('T_ML_COMMENT'))
  define ('T_ML_COMMENT', -1);

// 读取输入文件
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

// 写入缩减后的文件
file_put_contents('min_'.$file, $output);
```

核心是`token_get_all()`函数，它将PHP代码解析成可以唯一识别的单个 "原子"（tokens），然后根据需要忽略掉。

例如，它生成（对于这个例子，我使用了`Nette\Utils\Images`方法）。

<img src="{$baseUrl}/images/nette-image-minify.png" alt="最小化的代码">。
