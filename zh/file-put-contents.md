文件_put_contents
===============

> id: '7ec781c6-e3db-484d-8a49-0b93ab8252b1'
> slug:
> 	cs: file-put-contents
> 	zh: wen-jian-put-contents
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '78211a3696ebba56d559045f8a0ab9e4'

**file_put_contents**函数适用于自动写入文件。另外，你也可以使用<a href="/fopen">fopen()</a>，我不建议初学者使用。

样品
--------------------------

```php
$file = 'file.txt';
$content = '要保存到一个文件的内容。';

file_put_contents($file, $content);
```

file_put_contents有2个参数。

- `filename`写在哪里。
- 我们要写的 "文件的内容"。

> 注意：`file_put_contents()`会用最新的内容覆盖该文件。

注意覆盖的问题
--------------------------

如果你通过file_put_contents进行保存，要注意覆盖数据。该函数将删除所有当前内容，并以新内容取代。因此，如果你只是想追加文本，你可以用自己的脚本把它加到开头或结尾。

```php
$file = 'file.txt';
$content = '新的内容。';

$oldContent = file_get_contents($file);
file_put_contents($file, $content . $oldContent);
```

因此，首先打开文件，然后写入新的内容，然后再写入原来的内容......。

如果我们想在新内容之前添加旧内容，我们只需要稍微修改一下脚本。

```php
$file = 'file.txt';
$content = Nový obsah.';

$oldContent = file_get_contents($soubor);
file_put_contents($file, $oldContent . $content);
```
