Ende()
======

> id: '879c7c9a-a497-4080-865d-f15e6c9e8578'
> slug:
> 	cs: end
> 	de: ende
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '2d097dd1fae4e4f125d879c92bc2cfbe'

- Unterstützung von PHP 4, PHP 5
- Kurzbeschreibung: setzt den internen Zeiger eines Arrays auf dessen letztes Element.
- Anforderungen.

Beschreibung
--------------------------

Die Funktion `end()` setzt einen internen Array-Zeiger auf das letzte Element und gibt dessen Wert zurück.

Ähnliche Funktionen
--------------------------

- `Aktuell()`
- each()`
- `prev()`
- <a href="/zurücksetzen">zurücksetzen()</a>
- `Nächste()`

Beispiel
--------------------------

```php
echo end([
    'Apfel',
    'Banane',
    'Moosbeere',
]); // schreibt Cranberry aus
```



```php
$a = [];
$a[1] = 1;
$a[0] = 0;

echo end($a); // druckt 0
```

Parameter
--------------------------

| # | Typ | Beschreibung |
| --- | ------- | ----- |
| 1 | "Array" | Der Name des Arrays, mit dem gearbeitet werden soll.

Rückgabewerte
--------------------------

Gibt den Wert des letzten Elements oder `false` für ein leeres Array zurück.

Unterschiede zu früheren Versionen
--------------------------

Keine
