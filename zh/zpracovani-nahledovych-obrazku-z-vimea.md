处理来自Vimeo的缩略图和元信息
=================

> id: '8b8cbef0-210a-4883-be07-5004e8f43739'
> slug:
> 	cs: zpracovani-nahledovych-obrazku-z-vimea
> 	zh: chu-li-lai-zivimeo-de-suo-e-tu-he-yuan-xin-xi
> 
> publicationDate: '2020-09-19 17:32:31'
> mainCategoryId: a0143f3c-ac75-46dc-a514-d3c9417ded4e
> sourceContentHash: '93c29218f162fb5dc8d0f265968b0f8d'

当从Vimeo嵌入视频到一个页面时（作为HTML嵌入），我们可能经常想同时获得图像和其他有用的信息，如视频长度、完整的标题、作者等等。

幸运的是，Vimeo提供了一个简单的HTTP API，我们可以从中读取基于视频令牌的所有数据。

为了避免自己编写API，只需使用[ready package](https://github.com/baraja-core/vimeo-video-api)，它完全整合了API。

你用命令安装该软件包。

```shell
composer require baraja-core/vimeo-video-api
```

它很容易使用。你创建一个`\Baraja\VimeoAPI\VimeoVideoAPI`服务的实例，根据文档与Vimeo通信，调用`getInfo()`方法，传递视频令牌，并作为`VideoInfo`实体获得详细信息，从中可以读取所有可用信息（并非每个视频都有所有信息）。

这样一来，你甚至可以查询到私人和公开不可用的视频。但你总是需要知道他们的令牌。

列出所有可用的信息
---------

使用该库的基本方法是这样的。

```php
$api = new \Baraja\VimeoAPI\VimeoVideoAPI;

$token = 0; // 视频令牌为整数
$info = $api->getInfo($token);

echo var_dump($info); //列出了所有的东西

// 打印视频的长度，以秒为单位。
echo '该视频的长度为。' . $info->getDuration();
```

`$info`变量存储了所有关于一个特定视频的描述性信息。所有可用方法的概述[可在实施中找到](https://github.com/baraja-core/vimeo-video-api/blob/master/src/VideoInfo.php)。
