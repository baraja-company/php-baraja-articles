Validierung und Formatierung von Telefonnummern
===============================================

> id: bbfe35c3-3a8c-4fe5-a9bd-5bff0fc234e0
> slug:
> 	cs: telefonni-cisla
> 	de: validierung-und-formatierung-von-telefonnummern
> 
> publicationDate: '2021-06-18 09:20:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '224b3857a6bb898d9d322499cd1d3757'

Es gibt keine einfache Möglichkeit, Telefonnummern in PHP zu validieren und zu formatieren, also habe ich eine einfache Bibliothek geschrieben, die keine Abhängigkeiten hat, aber trotzdem diese Aufgabe übernehmen kann.

Ziel ist es, das Format einer Telefonnummer zu überprüfen oder sie in eine einfache kanonische Form zu konvertieren (die immer gültig ist).

Installation von
---------

Einfach durch den Komponisten:

```txt
$ composer require baraja-core/phone-number
```

Oder laden Sie das Paket [Download auf GitHub](https://github.com/baraja-core/phone-number) herunter.

Wie man die Bibliothek benutzt
----------

Das Prinzip dieses Tools basiert auf der Formatierung und Validierung von Telefonnummern, die vom Benutzer oder aus Quellen stammen, über die Sie keine Kontrolle haben.

Die häufigste Verwendung ist die Korrektur der Formatierung von Telefonnummern:

```php
$original = '+420 777123456';
$formatted = \Baraja\PhoneNumber\PhoneNumberFormatter::fix($original);

echo $original . '<br>';
echo $formatted;
```

Die Funktion korrigiert die Zahlenformatierung und gibt die Zeichenfolge "+420 777 123 456" zurück.

Wird vom Benutzer kein Präfix angegeben, wird das Präfix "+420" angenommen. Sie können die Standardeinstellung mit dem zweiten Parameter ändern:

```php
// Rückgabe: +421 777 123 456
\Baraja\PhoneNumber\PhoneNumberFormatter::fix('+420 777123456', 421);
```

Der Telefoncode wird nur überschrieben, wenn der Benutzer ihn nicht eingibt und er nicht automatisch erkannt wird.

Eingabe- und Ausgabeformatierung
----------------------------

Die Eingabezeichenfolge kann (fast) beliebig aussehen. Der eingebaute Algorithmus kann automatisch ungültige Zeichen entfernen (z. B. schreiben manche Benutzer eine Notiz neben eine Telefonnummer, die automatisch entfernt wird). Sie müssen sich also keine Gedanken über die Formatierung der Eingabe machen, aber die Ausgabe wird immer konsistent sein.

Die Ausgabe sieht immer gleich aus (sie ist auf das kanonische Format normalisiert).

Das allgemeine Format ist:

```txt
   +420 777 123 456
     |  \_________/
  Prefix     |
      National number
```

Wenn Sie eine ungültige Eingabe machen (oder eine Eingabe, die nicht automatisch korrigiert werden kann), wird eine Ausnahme ausgelöst.

Fehlerbehebung
----------------

Wenn eine Zahl nicht sicher auf eine Basisform normalisiert werden kann oder nicht existiert, wird eine Ausnahme "InvalidArgumentException" ausgelöst.

Wenn Sie die Ausnahme in einen Booleschen Wert umwandeln möchten, verwenden Sie den integrierten Asset Validator:

```php
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('123'); // falsch
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('777123456'); // wahr
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777123456'); // wahr
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777 123 456'); // wahr
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 77 712 34 56'); // wahr
```
