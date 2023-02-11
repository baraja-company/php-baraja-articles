如何处理PHP脚本突然崩溃的问题
================

> id: '41edbf1b-3ca5-487f-a54c-fac9926bc3d2'
> slug:
> 	cs: senior-7-sessions
> 	zh: ru-he-chu-liphp-jiao-ben-tu-ran-beng-kui-de-wen-ti
> 
> publicationDate: '2023-02-11 14:30:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: bdd1e207ee9c0ba19c39f0b5c7d83c43

2016年底的一个故事，当时我真的被一个同事救了：在一个PHP应用程序中，你决定通过一个代理脚本来检查图片，除其他外，它可以根据传入的请求调整其尺寸和其他参数。作为优化的一部分，你也将生成的变体物理地保存到磁盘上。

然而，在生产操作中，你突然开始看到巨大的负载和成千上万的请求排队。每个用户的图像都是按顺序逐一加载的。页面刷新和链接点击都不起作用。该应用程序似乎完全冻结。只有等待一切处理才行。

可能是什么问题？我在文中列出了3条主要线索，以便能够快速搜索到问题。Hotfix有一个微不足道的解决方案。
