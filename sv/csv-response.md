Skicka en CSV-fil
=================

> id: '35c4f30c-8638-496b-82a7-1297a598fbdb'
> slug:
> 	cs: csv-response
> 	sv: skicka-en-csv-fil
> 
> publicationDate: '2022-12-18 11:10:00'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '582e41a6e41b568d09be08c482523df8'

När du skickar binära filer ska du alltid tänka på vilka HTTP-huvuden du ska välja. Om du skickar en CSV-fil (nästan idealiskt format för enkla texttabeller som kan bearbetas i Excel) är det lämpligt att använda `Content-Type: application/csv` i kodning `UTF-8`.

I vissa versioner av Excel finns det dock ett problem med UTF-8-kodning. För att se till att rätt kodning upptäcks måste vi infoga `UTF-8 BOM`, vilket är ett specialtecken ``xEF\xBB\xBF``` som talar om för klienten att det är UTF-8, eftersom det inte finns i någon annan kodning.

Skicka därför rubrikerna på följande sätt:

```php
header('Content-Type: application/csv; charset=utf-8');
header('Content-Disposition: bilaga; filnamn=' . date('d-m-y') . '_file.csv');
header('Pragma: no-cache');
echo "\xEF\xBB\xBF";
```
