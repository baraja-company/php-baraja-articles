cURLによるHTTPリクエスト情報の取得
=====================

> id: '310e5143-6b38-44c1-a1bf-de37f94ee17d'
> slug:
> 	- null
> 	ja: curlniyoruhttprikuesuto-qing-baono-qu-de
>
> cs: curl-getinfo
> publicationDate: '2022-07-06 19:50:00'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '36748f0e9ced28da3da80155a0253140'

PHP の関数 `curl_getinfo()` は、実行された cURL リクエストに関する詳細な情報を提供します。各フィールドの意味について説明します。

使用例
---------------

curl_init()` からのコンテキストの結果に対して、この関数を呼び出します。

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

数値の表
--------------

`curl_getinfo()` 関数は、個々のキーと値を取得することができる連想配列を返します。

| キー                        | 値の例                        | 説明                                                   |
|---------------------------|----------------------------|------------------------------------------------------|
| `url`                     | 'https://baraja.cz/'       | ダウンロードしたURLです。                                       |
| `content_type`            | 'text/html; charset=utf-8' | 使用されるエンコードとコンテンツタイプ（ターゲットサーバーが主張する）。                 |
| `http_code`               | 200                        | HTTPステータスコードが返されました。                                 |
| `header_size`             | 462                        | HTTPリクエストヘッダーサイズ（バイト）。                               |
| `request_size`            | 47                         | リクエストサイズ。                                            |
| `filetime`                | -1                         | ファイルの時間（サーバーの主張）。                                    |
| `ssl_verify_result`       | 0                          | SSLチェック。                                             |
| `redirect_count`          | 0                          | ターゲットドキュメントに到達するまでにリダイレクトされた回数。                      |
| `total_time`              | 0.233384                   | 応答を待つために費やされた合計時間です。秒単位で表示されます。                      |
| `namelookup_time`         | 0.021608                   | DNSレコード上でドメインを解決するのにかかった時間です。秒単位で指定する。               |
| `connect_time`            | 0.035031                   | 接続先サーバーとの接続を確立するための時間です。秒単位で指定する。                    |
| `pretransfer_time`        | 0.187275                   | データ転送に必要な時間です。秒単位で指定する。                              |
| `size_upload`             | 0.0                        | アップロードされたデータのサイズ（バイト）です。                             |
| `size_download`           | 0.0                        | ダウンロードしたデータのサイズ(バイト)。                                |
| `speed_download`          | 0.0                        | ダウンロード速度（バイト毎秒）。                                     |
| `speed_upload`            | 0.0                        | アップロードの速度（バイト/秒）。                                    |
| `download_content_length` | 15522.0                    | ダウンロードしたデータのサイズ(バイト)。                                |
| `upload_content_length`   | -1.0                       | アップロードされたデータのサイズ（バイト）です。                             |
| `starttransfer_time`      | 0.233354                   | TTFB (Time To First Byte) 値を秒単位で示す。                  |
| `redirect_time`           | 0.0                        | 正規のコンテンツをダウンロードするためにリダイレクトに費やされた時間。                  |
| `redirect_url`            | `''`                       | 正規のURLとリダイレクト先。                                      |
| `primary_ip`              | '76.76.21.21'              | どのIPからコンテンツがダウンロードされたのか。                             |
| `certinfo`                | array (0)                  | ターゲットサイトの証明書に関する詳細情報。                                |
| `primary_port`            | 443                        | 使用するネットワークポート（80はHTTP、443はHTTPSを意味する）。               |
| `local_ip`                | '192.168.0.186'            | リクエストを送信したマシンのローカルIPアドレスです。                          |
| `local_port`              | 56568                      | リクエストが送信されたローカルマシンのポートです。                            |
| `http_version`            | 3                          | HTTPプロトコルのバージョンです。                                   |
| `protocol`                | 2                          | 使用したプロトコルのコード。                                       |
| `ssl_verifyresult`        | 0                          | SSL検証結果。                                             |
| `scheme`                  | 'HTTPS'                    | URLの先頭にあるプロトコル。                                      |
| `appconnect_time_us`      | 186220                     | ターゲットサーバーに接続するまでの時間です。マイクロ秒単位で指定する。                  |
| `connect_time_us`         | 35031                      | 接続先サーバーに接続するまでの時間。マイクロ秒単位で指定する。                      |
| `namelookup_time_us`      | 21608                      | DNSレコードでドメインを書き換えるのに必要な時間です。マイクロ秒単位で指定する。            |
| `pretransfer_time_us`     | 187275                     | データ転送に要した時間です。マイクロ秒単位で指定する。                          |
| `redirect_time_us`        | 0                          | 正規のコンテンツをダウンロードするためにリダイレクトするのにかかった時間。マイクロ秒単位で表示されます。 |
| `starttransfer_time_us`   | 233354                     | TTFB (Time To First Byte)時間の値を示す。マイクロ秒単位で            |
| `total_time_us`           | 233384                     | 応答を待つために費やされた合計時間です。マイクロ秒単位で指定する。                    |

一部のキーは常時使用できない場合があります。値を読み出す前に、必ずキーの存在と値の有効性を確認する。
