包括（折页的片断）。
==========

> id: '4984832e-c11f-4e9e-8d3b-60561685389d'
> slug:
> 	cs: include-soubor
> 	zh: bao-kuo-zhe-ye-de-pian-duan
> 
> publicationDate: '2019-08-23 15:06:33'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: aea5867cf788e623dddd3ced4ea24a7a

PHP最初是一种模板语言，它的诞生是为了使网页的各个部分能够容易地组合在一起。

支持的格式
-------------------

折叠在文本形式下工作，所以建议使用相关的格式，如`.html`或`.md`。

当一个PHP文件被粘贴时，其内容会被执行，就像它们在被粘贴的地方实际存在一样。

折叠页面和插入普通内容
---------------------------------------------

通常，我们需要创建几个有共同内容的页面--例如，一个菜单。

在普通的HTML中，我们会首先创建一个带有菜单的页面，然后多次复制。但在PHP中，我们可以将整个过程自动化。

让我们有一个文件`menu.html`，菜单内容在那里，`index.php`，我们把内容和菜单放在那里。

一个简单的例子。

```php
<div class="页">
    <div class="内容">
        <?php
            include __DIR__
            . '/article/' . ($_GET['页'] ?? '索引') . '.html';
        ?>
    </div>
    <div class="menu">
                    include 'menu.html';
        ?>
    </div>
</div>
```

这个脚本自动插入`/article`目录中的页面内容，并根据用户的输入（URL参数`?page=...`）读取文件名。如果没有传递参数，就会使用`index.html`。

因此，URL可能看起来像，例如，`example.com?page=contacts`并加载`/article/contacts.html`。
