Odoslanie súboru CSV
====================

> id: '35c4f30c-8638-496b-82a7-1297a598fbdb'
> slug:
> 	cs: csv-response
> 	sk: odoslanie-suboru-csv
> 
> publicationDate: '2022-12-18 11:10:00'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '582e41a6e41b568d09be08c482523df8'

Pri odosielaní binárnych súborov si vždy premyslite, aké hlavičky HTTP zvoliť. V prípade odosielania súboru CSV (takmer ideálny formát pre jednoduché textové tabuľky, ktoré je možné spracovať v programe Excel) je vhodné použiť `Content-Type: application/csv` v kódovaní `UTF-8`.

V niektorých verziách programu Excel je však problém s kódovaním UTF-8. Aby sme sa uistili, že je rozpoznané správne kódovanie, musíme vložiť `UTF-8 BOM`, čo je špeciálny znak ``xEF\xBB\xBF``, ktorý klientovi hovorí, že ide o UTF-8, pretože v žiadnom inom kódovaní neexistuje.

Preto pošlite hlavičky takto:

```php
header('Content-Type: application/csv; charset=utf-8');
header('Content-Disposition: attachment; filename=' . date('d-m-y') . '_file.csv');
header('Pragma: no-cache');
echo "\xEF\xBB\xBF";
```
