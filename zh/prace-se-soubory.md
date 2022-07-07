与文件一起工作
=======

> id: '12330cfb-5729-419a-b2e4-cb3d1db998cc'
> slug:
> 	cs: prace-se-soubory
> 	zh: yu-wen-jian-yi-qi-gong-zuo
> 
> publicationDate: '2020-02-16 16:30:05'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '0f5b9e84a974285dcb63baafc912be5f'

在PHP中，有许多函数用于处理文件。

- <a href="/fopen">fopen()</a>，对磁盘上文件的低层次访问
- <a href="/file-get-contents">file_get_contents()</a>，检索一个文件或URL的内容。
- <a href="/file-put-contents">file_put_contents()</a>，将一个字符串保存到一个本地文件。

磁盘功能
--------------

- `unlink($path)`, 删除文件
- `copy($from, $to)`, 将一个文件从一个地方复制到另一个地方（可以从一个URL复制到本地磁盘）。
- `rename()`, 重新命名或移动磁盘上的文件
- `chmod()`，改变读取、写入或执行一个文件的权限
