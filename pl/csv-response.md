Wysyłanie pliku CSV
===================

> id: '35c4f30c-8638-496b-82a7-1297a598fbdb'
> slug:
> 	cs: csv-response
> 	pl: wysylanie-pliku-csv
> 
> publicationDate: '2022-12-18 11:10:00'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '582e41a6e41b568d09be08c482523df8'

Wysyłając pliki binarne, zawsze należy zastanowić się, jakie nagłówki HTTP wybrać. W przypadku wysyłania pliku CSV (niemal idealny format dla prostych tabel tekstowych, które mogą być przetwarzane przez Excel) przydatny jest `Content-Type: application/csv`, w kodowaniu `UTF-8`.

Jednak w niektórych wersjach Excela występuje problem z kodowaniem UTF-8. Aby mieć pewność, że wykryte zostanie prawidłowe kodowanie, musimy wstawić `UTF-8 BOM`, czyli specjalny znak `xEF`, który mówi klientowi, że jest to UTF-8, ponieważ nie występuje w żadnym innym kodowaniu.

Dlatego wyślij nagłówki w następujący sposób:

```php
header('Content-Type: application/csv; charset=utf-8');
header('Content-Disposition: attachment; filename=.' . date('d-m-y') . '_file.csv');
header('Pragma: no-cache');
echo "\...";
```
