发送CSV文件
=======

> id: '35c4f30c-8638-496b-82a7-1297a598fbdb'
> slug:
> 	cs: csv-response
> 	zh: fa-songcsv-wen-jian
> 
> publicationDate: '2022-12-18 11:10:00'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '582e41a6e41b568d09be08c482523df8'

在发送二进制文件时，一定要考虑选择什么样的HTTP头信息。如果发送CSV文件（几乎是简单文本表格的理想格式，可由Excel处理），`Content-Type: application/csv', `UTF-8'编码的文件是有用的。

然而，在某些版本的Excel中，UTF-8编码存在问题。为了确保检测到正确的编码，我们需要插入 "UTF-8 BOM"，这是一个特殊的字符`xEFxBBxBF`，它告诉客户端是UTF-8，因为它不存在于任何其他编码中。

因此，发送头信息如下。

```php
header('Content-Type: application/csv; charset=utf-8');
header('Content-Disposition: 附件; filename=' . date('d-m-y') . '_file.csv');
header('Pragma: no-cache');
echo "\xEF\xBB\xBF";
```
