PHP-Funktion fopen()
====================

> id: '733ba757-1d5f-4717-a088-5ddabe730838'
> slug:
> 	cs: fopen
> 	de: php-funktion-fopen
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '6b791877e57d88840d603dea69967b69'

Die Funktion `fopen()` steht für den Low-Level-Zugriff auf Dateien auf der Festplatte.

Der Programmierer muss alles selbst machen (Datei öffnen, Daten lesen, neue Daten schreiben, Datei schließen).

Wenn Sie nur schnell Dateien lesen und schreiben müssen, gibt es einfachere Optionen:

- <a href="/file-get-contents">file_get_contents()</a> - Lesen aus einer Datei
- <a href="/file-put-contents">file_put_contents()</a> - Schreiben in eine Datei

Grundlegende Verwendung
----------------

```php
$text = 'Jeder Text, der gespeichert wird...';

$file = fopen('Datei.html', 'a+'); // Öffnet Datei und Modus

fwrite($file, $text); // Speichert in einer Datei
fclose($file);        // Schließt die Datei
```

> Wenn wir eine Datei zum Lesen öffnen und sie nicht geschlossen wird, kann kein anderer Prozess auf sie zugreifen!

Arten von Dateiverarbeitungsmodi
----------------------------

Wir können mit Dateien in verschiedenen Modi arbeiten, die Auskunft über die Zugriffsrechte geben.

Wenn wir zum Beispiel eine Datei nur zum Lesen öffnen wollen, dann ist der Modus "r" ausreichend.

Wenn wir die Datei zum Schreiben öffnen, wird sie auf der Festplatte als "offen" markiert, und ein anderer Prozess (Skript) kann nicht in sie schreiben, bis wir sie wieder schließen. Dadurch wird sichergestellt, dass die Datei beim Schreiben nicht beschädigt wird.

| Modus | Bedeutung |
|-------|--------|
| `und` | Öffnet die Datei, wenn sie nicht existiert, wird sie erstellt |
| `a+` | Öffnet eine Datei, um Daten hinzuzufügen oder zu lesen; wenn sie nicht existiert, wird sie erstellt |
| "r" | Schreibgeschützt öffnen |
| | `r+` | Öffnen zum Lesen und Schreiben |
| `w` | Zum Schreiben öffnen, ursprüngliche Daten werden gelöscht und durch neue Daten ersetzt, wenn sie nicht existieren, werden sie neu erstellt |
| `w+` | Öffnen zum Schreiben und Lesen, ursprüngliche Daten werden gelöscht und durch neue ersetzt, wenn sie nicht existieren, werden sie neu erstellt |
