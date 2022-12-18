CSVファイルを送信する
============

> id: '35c4f30c-8638-496b-82a7-1297a598fbdb'
> slug:
> 	cs: csv-response
> 	ja: csvfairuwo-song-xinsuru
> 
> publicationDate: '2022-12-18 11:10:00'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '582e41a6e41b568d09be08c482523df8'

バイナリファイルを送信する場合、どのようなHTTPヘッダを選択するかを常に考えておく必要があります。CSV ファイル (Excelで処理できるシンプルなテキストテーブルとしてほぼ理想的なフォーマット) を送る場合、 `Content-Type: application/csv`, `UTF-8` エンコーディングが有効です。

しかし、Excelの一部のバージョンでは、UTF-8のエンコードに問題があります。正しいエンコーディングを検出するために、`UTF-8 BOM`を挿入する必要があります。これは特殊文字 ``xEFxBBxBF`` で、他のエンコーディングでは存在しないため、クライアントにUTF-8であることを知らせます。

したがって、以下のようにヘッダを送信します。

```php
header('Content-Type: application/csv; charset=utf-8');
header('Content-Disposition: attachment; filename=。' . date('エムワイ') . '_file.csv');
header('プラグマ：no-cache');
echo "\xEFxBBxBF";
```
