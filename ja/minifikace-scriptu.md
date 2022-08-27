PHPスクリプトの自動最小化
==============

> id: f9225faf-f881-4f7d-8c0f-bab4cfea9cb9
> slug:
> 	cs: minifikace-scriptu
> 	ja: phpsukuriputono-zi-dong-zui-xiao-hua
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '1a465671563e1a2d05dc17b93bb1c442'

時には、大きなPHPスクリプトを縮小し、いくつかのスクリプトを1つのファイルに圧縮する必要があります。これは、インターネットに公開するライブラリを作っていて、誰にも邪魔されたくない場合や、頻繁にコピーする便利なスクリプトで、あまり多くのデータを転送したくない場合などに有効です。

解決策として考えられるのは、コードの最小化です。

このためのオンラインツールを用意しました（コードを貼り付けるだけで、すぐにminifyされたバージョンが返ってきます）。

ミニファミリーのコアは、この最小値まで減らすことができる。

```php
$file = 'StaticClass.php';

// DgxのPHPシュリンカー

// PHP 4 と 5 の互換性
if (!defined('T_DOC_COMMENT'))
  define ('T_DOC_COMMENT', -1);

if (!defined('T_ML_COMMENT'))
  define ('T_ML_COMMENT', -1);

// 入力ファイルの読み込み
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

// 縮小したファイルを書き込む
file_put_contents('{cH00ffff}私達は、私達のために'.$file, $output);
```

核となるのは `token_get_all()` 関数で、PHP コードを一意に識別できる個々の「アトム」(トークン) にパースし、必要に応じて無視できるようにします。

例えば、次のようなものが生成されます（例では `NetteUtils FilterImages` メソッドを使用しました）。

<img src="{$baseUrl}/images/nette-image-minify.png" alt="ミニファイコード">。
