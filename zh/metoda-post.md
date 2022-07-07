通过POST方法接收数据
============

> id: c2f273fa-7730-4521-82d0-1b3096269fac
> slug:
> 	cs: metoda-post
> 	zh: tong-guopost-fang-fa-jie-shou-shu-ju
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '51a30cb759cdbb184ee5bb0bf3070af1'

通过POST发送数据与<a href="/method-get">GET</a>有很大的不同，它更安全，文本可以更长，其值不能被确定，除非是通过表单或标头（你不会错误地得到）。

来源
--------------------------

Source与<a href="/method-get">GET</a>方法没有什么不同。这几乎是一样的，只是参数不显示在URL中，只有文件名是可见的。

```php
echo $_POST['文章'] ?? '';
```

特点、优势和劣势
--------------------------

- 参数不能被链接到，但必须提交表格
- 不能被索引（与上一点有关）
- 比<a href="/method-get">GET</a>更安全（数据以隐蔽的方式发送，数值不会显示在历史中）。
- 不要与SSL混淆，POST是不加密的。
