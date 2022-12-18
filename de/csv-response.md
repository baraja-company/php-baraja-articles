Versenden einer CSV-Datei
=========================

> id: '35c4f30c-8638-496b-82a7-1297a598fbdb'
> slug:
> 	cs: csv-response
> 	de: versenden-einer-csv-datei
> 
> publicationDate: '2022-12-18 11:10:00'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '582e41a6e41b568d09be08c482523df8'

Wenn Sie Binärdateien versenden, sollten Sie immer überlegen, welche HTTP-Header Sie wählen. Im Falle der Übermittlung einer CSV-Datei (nahezu ideales Format für einfache Texttabellen, die von Excel verarbeitet werden können) ist `Content-Type: application/csv` in der Kodierung `UTF-8` nützlich.

In einigen Versionen von Excel gibt es jedoch ein Problem mit der UTF-8-Kodierung. Um sicherzustellen, dass die richtige Kodierung erkannt wird, müssen wir das "UTF-8 BOM" einfügen, ein spezielles Zeichen "xEF\xBB\xBF", das dem Client mitteilt, dass es sich um UTF-8 handelt, da es in keiner anderen Kodierung existiert.

Senden Sie daher die Kopfzeilen wie folgt:

```php
header('Inhalt-Typ: application/csv; charset=utf-8');
header('Inhalt-Disposition: attachment; filename=' . date('d-m-y') . '_file.csv');
header('Pragma: no-cache');
echo "\xEF\xBB\xBF";
```
