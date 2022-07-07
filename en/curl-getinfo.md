Getting HTTP request information via cURL
=========================================

> id: '310e5143-6b38-44c1-a1bf-de37f94ee17d'
> slug:
> 	- null
> 	en: getting-http-request-information-via-curl
> 
> cs: curl-getinfo
> publicationDate: '2022-07-06 19:50:00'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '36748f0e9ced28da3da80155a0253140'

The PHP function `curl_getinfo()` provides detailed information about the executed cURL request. This article explains the meaning of each field.

Example usage
---------------

Call the function over the result of the context from `curl_init()`:

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

Table of values
--------------

The `curl_getinfo()` function returns an associative array from which individual keys and values can be retrieved.

| Key | Example value | Explanation |
|------|-----------------|------------|
| `url` | 'https://baraja.cz/' | Downloaded URL. |
| `content_type` | 'text/html; charset=utf-8' | Encoding and content type used (claimed by the target server). |
| `http_code` | 200 | HTTP status code returned. 200 means OK. |
| `header_size` | 462 | HTTP request header size in bytes. |
| `request_size` | 47 | Request size. |
| | `filetime` | -1 | File time (server claims). |
| `ssl_verify_result` | 0 | SSL check. |
| | `redirect_count` | 0 | Number of redirects before reaching the target document.
| `total_time` | 0.233384 | Total time spent waiting for response. Given in seconds. |
| `namelookup_time` | 0.021608 | Time taken to resolve domain over DNS records. Specified in seconds. |
| | `connect_time` | 0.035031 | Time to establish a connection to the destination server. Specified in seconds. |
| | `pretransfer_time` | 0.187275 | Time required to transfer data. Specified in seconds. |
| `upload_size` | 0.0 | Size of the uploaded data in bytes. |
| `size_download` | 0.0 | Downloaded data size in bytes. |
| `speed_download` | 0.0 | Download speed in bytes per second. |
| `speed_upload` | 0.0 | Upload speed in bytes per second. |
| `download_content_length` | 15522.0 | Downloaded data size in bytes. |
| `upload_content_length` | -1.0 | Size of uploaded data in bytes. |
| `starttransfer_time` | 0.233354 | Indicates the TTFB (Time To First Byte) value in seconds. |
| `redirect_time` | 0.0 | Time spent redirecting to download canonical content.
| | `redirect_url` | `''` | canonical URL and redirect destination. |
| `primary_ip` | '76.76.21.21' | From which IP the content was downloaded. |
| `certinfo` | array (0) | More details about the certificate of the target site. |
| `primary_port` | 443 | The network port used (80 means HTTP, 443 means HTTPS). |
| `local_ip` | '192.168.0.186' | Local IP address of the machine that sent the request. |
| `local_port` | 56568 | Port of the local machine from which the request was sent. |
| `http_version` | 3 | HTTP protocol version. |
| | `protocol` | 2 | Code of the protocol used. |
| `ssl_verifyresult` | 0 | SSL verification result. |
| `scheme` | 'HTTPS' | Protocol at the beginning of the URL. |
| `appconnect_time_us` | 186220 | Time to connect to the target server. Specified in nanoseconds. |
| | `connect_time_us` | 35031 | Time to connect to the destination server. Specified in nanoseconds. | |
| | `namelookup_time_us` | 21608 | Time required to rewrite the domain via DNS records. Specified in nanoseconds. |
| | `pretransfer_time_us` | 187275 | Time taken to transfer data. Specified in nanoseconds. |
| `redirect_time_us` | 0 | Time spent redirecting to download canonical content. Given in nanoseconds. |
| `starttransfer_time_us` | 233354 | Indicates the value of the TTFB (Time To First Byte) time. In nanoseconds. |
| | `total_time_us` | 233384 | Total time spent waiting for a response. Specified in nanoseconds. |

Some keys may not always be available. Always verify the existence of the key and the validity of the value before reading the value.
