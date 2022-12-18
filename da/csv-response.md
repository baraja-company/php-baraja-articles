Afsendelse af en CSV-fil
========================

> id: '35c4f30c-8638-496b-82a7-1297a598fbdb'
> slug:
> 	cs: csv-response
> 	da: afsendelse-af-en-csv-fil
> 
> publicationDate: '2022-12-18 11:10:00'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '582e41a6e41b568d09be08c482523df8'

Når du sender binære filer, skal du altid overveje, hvilke HTTP-headere du skal vælge. I tilfælde af at sende en CSV-fil (næsten ideelt format for simple teksttabeller, som kan behandles af Excel) er det nyttigt med `Content-Type: application/csv`, i `UTF-8`-kodning.

I nogle versioner af Excel er der dog et problem med UTF-8-kodning. For at sikre, at den korrekte kodning registreres, skal vi indsætte `UTF-8 BOM`, som er et særligt tegn ``xEF\xBB\xBF```, der fortæller klienten, at det er UTF-8, da det ikke findes i nogen anden kodning.

Derfor skal du sende overskrifterne som følger:

```php
header('Content-Type: application/csv; charset=utf-8');
header('Content-Disposition: attachment; filnavn=' . date('d-m-y') . '_file.csv');
header('Pragma: no-cache');
echo "\xEF\xBB\xBF";
```
