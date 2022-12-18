Invio di un file CSV
====================

> id: '35c4f30c-8638-496b-82a7-1297a598fbdb'
> slug:
> 	cs: csv-response
> 	it: invio-di-un-file-csv
> 
> publicationDate: '2022-12-18 11:10:00'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '582e41a6e41b568d09be08c482523df8'

Quando si inviano file binari, bisogna sempre pensare a quali intestazioni HTTP scegliere. In caso di invio di un file CSV (formato quasi ideale per tabelle di testo semplici, che possono essere elaborate da Excel), è utile `Content-Type: application/csv`, con codifica `UTF-8`.

Tuttavia, in alcune versioni di Excel esiste un problema con la codifica UTF-8. Per assicurarsi che venga rilevata la codifica corretta, occorre inserire la `UTF-8 BOM', che è un carattere speciale ``xEF\xBB\xBF'' che indica al client che si tratta di UTF-8, poiché non esiste in nessun'altra codifica.

Pertanto, inviare le intestazioni come segue:

```php
header('Tipo di contenuto: application/csv; charset=utf-8');
header('Contenuto-Disposizione: allegato; nome del file=' . date('d-m-y') . '_file.csv');
header('Pragma: no-cache');
echo "\´exEF´xBB´xBF";
```
