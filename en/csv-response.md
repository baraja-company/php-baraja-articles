Sending a CSV file
==================

> id: '35c4f30c-8638-496b-82a7-1297a598fbdb'
> slug:
> 	cs: csv-response
> 	en: sending-a-csv-file
> 
> publicationDate: '2022-12-18 11:10:00'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '582e41a6e41b568d09be08c482523df8'

When sending binary files, always think about what HTTP headers to choose. In case of sending a CSV file (almost ideal format for simple text tables, which can be processed by Excel), `Content-Type: application/csv`, in `UTF-8` encoding is useful.

However, in some versions of Excel there is a problem with UTF-8 encoding. To make sure that the correct encoding is detected, we need to insert the `UTF-8 BOM`, which is a special character ``xEF\xBB\xBF`` that tells the client that it is UTF-8, since it does not exist in any other encoding.

Therefore, send the headers as follows:

```php
header('Content-Type: application/csv; charset=utf-8');
header('Content-Disposition: attachment; filename=' . date('d-m-y') . '_file.csv');
header('Pragma: no-cache');
echo "\xEF\xBB\xBF";
```
