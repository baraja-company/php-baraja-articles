通过cURL获取HTTP请求信息
================

> id: '310e5143-6b38-44c1-a1bf-de37f94ee17d'
> slug:
> 	- null
> 	zh: tong-guocurl-huo-quhttp-qing-qiu-xin-xi
> 
> cs: curl-getinfo
> publicationDate: '2022-07-06 19:50:00'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '36748f0e9ced28da3da80155a0253140'

PHP函数`curl_getinfo()`提供了关于执行的cURL请求的详细信息。这篇文章解释了每个字段的含义。

使用实例
---------------

在`curl_init()`的上下文结果上调用该函数。

```php
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, 'https://baraja.cz');
curl_setopt($ch, CURLOPT_HEADER, 0);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 0);
curl_setopt($ch, CURLOPT_NOBODY, 1);
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 0);
curl_exec($ch);
$info = curl_getinfo($ch);
curl_close($ch);

dump($info);
```

价值表
--------------

`curl_getinfo()`函数返回一个关联数组，可以从中获取各个键和值。

| 关键 | 示例值 | 解释 |
|------|-----------------|------------|
| `url` | 'https://baraja.cz/' | 下载的URL。 |
| `content_type` | 'text/html; charset=utf-8' | 使用的编码和内容类型（由目标服务器声称）。
| `http_code` | 200 | 返回的HTTP状态代码。200表示OK。
| `header_size` | 462 | HTTP请求头大小（字节）。
| `request_size` | 47 | 请求大小。
| `filetime` | -1 | 文件时间（服务器声称）。
| `ssl_verify_result` | 0 | SSL检查。
| `redirect_count` | 0 | 到达目标文件前的重定向次数。
| `total_time` | 0.233384 | 等待响应的总时间。以秒为单位。
| `namelookup_time` | 0.021608 | 通过DNS记录解决域名的时间。以秒为单位指定。
| `connect_time` | 0.035031 | 与目标服务器建立连接的时间。以秒为单位指定。
| `pretransfer_time` | 0.187275 | 传输数据所需时间。以秒为单位指定。
| `upload_size` | 0.0 | 上传的数据大小（字节）。
| `size_download` | 0.0 | 下载的数据大小为字节。
| `speed_download` | 0.0 | 下载速度，以每秒钟的字节数计算。
| `speed_upload` | 0.0 | 上传速度，单位为每秒字节数。
| `download_content_length` | 15522.0 | 下载的数据大小为字节数。
| `upload_content_length` | -1.0 | 上传的数据大小（字节）。
| `starttransfer_time` | 0.233354 | 表示TTFB(Time To First Byte)值，单位是秒。
| `redirect_time` | 0.0 | 重定向下载规范内容的时间。
| `redirect_url'| `''| 规范的URL和重定向目的地。
| `primary_ip` | '76.76.21.21' | 从哪个IP下载的内容。
| `certinfo` | 数组 (0) | 关于目标站点证书的更多细节。
| `primary_port` | 443 | 使用的网络端口（80表示HTTP，443表示HTTPS）。
| `local_ip` | '192.168.0.186' | 发送请求的机器的本地IP地址。
| `local_port` | 56568 | 发送请求的本地机器的端口。
| `http_version` | 3 | HTTP协议版本。
| `protocol` | 2 | 使用的协议代码。
| `ssl_verifyresult` | 0 | SSL验证结果。
| `scheme` | 'HTTPS' | 协议在URL的开头。
| `appconnect_time_us` | 186220 | 连接到目标服务器的时间。以纳秒为单位指定。
| `connect_time_us` | 35031 | 连接到目标服务器的时间。以纳秒为单位指定。
| `namelookup_time_us` | 21608 | 通过DNS记录重写域名需要的时间。以纳秒为单位指定。
| `pretransfer_time_us` | 187275 | 传输数据的时间。以纳秒为单位指定。
| `redirect_time_us` | 0 | 重定向下载规范内容的时间。以纳秒为单位。
| `starttransfer_time_us` | 233354 | 表示TTFB（Time To First Byte）时间的值。以纳秒为单位。
| `total_time_us` | 233384 | 等待回复的总时间。以纳秒为单位指定。

有些钥匙可能并不总是可用。在读取值之前，一定要验证键的存在和值的有效性。
