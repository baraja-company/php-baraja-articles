Arbeiten mit Dateien
====================

> id: '12330cfb-5729-419a-b2e4-cb3d1db998cc'
> slug:
> 	cs: prace-se-soubory
> 	de: arbeiten-mit-dateien
> 
> publicationDate: '2020-02-16 16:30:05'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '0f5b9e84a974285dcb63baafc912be5f'

In PHP gibt es eine Reihe von Funktionen für die Arbeit mit Dateien.

- <a href="/fopen">fopen()</a>, Low-Level-Zugriff auf Dateien auf der Festplatte
- <a href="/file-get-contents">file_get_contents()</a>, Abrufen des Inhalts einer Datei oder einer URL
- <a href="/file-put-contents">file_put_contents()</a>, Speichern einer Zeichenkette in einer lokalen Datei.

Funktionen der Festplatte
--------------

- `unlink($pfad)`, Datei löschen
- `copy($from, $to)`, kopiert eine Datei von einem Ort zu einem anderen (kann von einer URL auf die lokale Festplatte sein)
- `rename()`, Umbenennen oder Verschieben einer Datei auf der Festplatte
- `chmod()`, Rechte zum Lesen, Schreiben oder Ausführen einer Datei ändern
