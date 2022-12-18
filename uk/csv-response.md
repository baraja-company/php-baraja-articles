Відправлення файлу CSV
======================

> id: '35c4f30c-8638-496b-82a7-1297a598fbdb'
> slug:
> 	cs: csv-response
> 	uk: vidpravlennya-fajlu-csv
> 
> publicationDate: '2022-12-18 11:10:00'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '582e41a6e41b568d09be08c482523df8'

При відправці бінарних файлів завжди думайте про те, які HTTP-заголовки вибирати. У разі надсилання файлу у форматі CSV (майже ідеальний формат для простих текстових таблиць, які можуть бути оброблені Excel), бажано вказати `Content-Type: application/csv`, у кодуванні `UTF-8`.

Однак, в деяких версіях Excel існує проблема з кодуванням UTF-8. Для того, щоб переконатися, що визначається правильне кодування, нам потрібно вставити `UTF-8 BOM`, який являє собою спеціальний символ `xEF\xBB\xBF`, який повідомляє клієнту, що це UTF-8, так як в інших кодуваннях його не існує.

Тому надсилайте заголовки наступним чином:

```php
header('Content-Type: application/csv; charset=utf-8');
header('Content-Disposition: attachment; filename=' . date('d-m-y') . '_file.csv');
header('Прагма: без кешу');
echo "\xEF\xBB\xBF";
```
