获取所有已加载文件的列表
============

> id: '72716cbc-90a6-4870-848b-125e8430707f'
> slug:
> 	cs: ziskani-seznamu-vsech-nactenych-souboru
> 	zh: huo-qu-suo-you-yi-jia-zai-wen-jian-de-lie-biao
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '7e896aa0e501d0fe33dc6595c1bfb43e'

在调试更复杂的应用程序时，有时会发生我不知道所有的文件都被加载了，是否有什么东西丢失了。

如果你使用Composer或任何其他类型的<a href="/autoloading-trid">类自动加载</a>，你可能不知道这个问题。然而，在调试其他开发者的旧应用程序时，这可能是一个比较常见的情况。

获取所有加载的文件可以用`get_included_files()`函数来完成，它以绝对路径字符串数组的形式返回。

在开发中，加载大量的文件是很常见的（例如，即使是这个相对简单的博客也使用了近160个文件）。不过，大多数时候，大体积并不重要，因为文件的内容是从OPCache中检索的，OPCache为批量读取进行了优化。
