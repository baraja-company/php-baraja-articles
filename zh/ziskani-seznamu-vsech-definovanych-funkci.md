获取所有定义的函数列表
===========

> id: '99ed7887-33c8-44d7-b0cb-ac37b2336f48'
> slug:
> 	cs: ziskani-seznamu-vsech-definovanych-funkci
> 	zh: huo-qu-suo-you-ding-yi-de-han-shu-lie-biao
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '9dbffadf6cf6473eedadbc0bf7eccbb4'

有时，获取当前环境中所有可用功能的列表是很有用的。当我们在管理别人的服务器时，尤其如此，我们需要了解自己的情况。

函数列表可以通过调用`get_defined_functions()`函数获得，该函数以数组的形式返回数据。

```txt
[
   internal => [
      ...,
   ],
   user => [
      ...,
   ]
]
```

功能列表分为两个大的列表。

- 内部 "函数是由PHP本身和已安装的扩展所定义的。
- 用户（`用户`）函数是由用户代码本身定义的。这些是我们写进源代码的任何函数，或包含在已安装的库中的函数。

这个列表可以很好地用于调试一个应用程序。
