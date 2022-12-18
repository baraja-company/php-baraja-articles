Odeslání CSV souboru
====================

> id: "35c4f30c-8638-496b-82a7-1297a598fbdb"
> slug:
> 	cs: csv-response
> 
> publicationDate: "2022-12-18 11:10:00"
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1

Při odesílání binárních souborů vždy přemýšlejte, jaké HTTP hlavičky zvolit. V případě odeslání CSV souboru (téměř ideální formát pro jednoduché textové tabulky, které umí zpracovat i Excel) se hodí `Content-Type: application/csv`, v kódování `UTF-8`.

V některých verzích Excelu však dochází k problému s kódováním UTF-8. Abychom si detekci správného kódování pojistili, je potřeba do souboru vložit tzv. `UTF-8 BOM`, což je speciální znak `"\xEF\xBB\xBF"`, který klientovi říká, že jde o UTF-8, jelikož v jiném kódování neexistuje.

Hlavičky proto pošlete takto:

```php
header('Content-Type: application/csv; charset=utf-8');
header('Content-Disposition: attachment; filename=' . date('d-m-Y') . '_file.csv');
header('Pragma: no-cache');
echo "\xEF\xBB\xBF";
```
