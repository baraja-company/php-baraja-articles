Überblick über die Kenntnisse der Webentwicklung
================================================

> id: e5e9613c-66fe-4d5e-a686-a182aab8061a
> slug:
> 	cs: znalosti
> 	de: ueberblick-ueber-die-kenntnisse-der-webentwicklung
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7284b1fc11b40ddb7628995070198a1

Das Wissen, wie man eine Website erstellt und dann umfassend pflegt, ist nicht nur eine Frage der Erstellung. Auf dem Weg dorthin gibt es viele Hürden, und es ist gut, wenn man zumindest eine grundlegende Vorstellung von den einzelnen Punkten hat. Als ich anfing, wusste ich nicht wirklich, was es alles zu lernen gab. Diese Seite dient als Wegweiser durch die Themen, die ich nach und nach lernen musste, um die Webentwicklung zu verstehen und mit den häufigsten Situationen umgehen zu können.

Server-Verwaltung
--------------

> Ein Webserver ist ein Computer, auf dem das Internet läuft. Wenn ein Benutzer eine Seite aufruft, sendet der Webserver die Seite an den Benutzer.
>
> Im Moment (2021) macht es keinen Sinn mehr, kostenloses Hosting zu nutzen, wenn man sich ernsthaft mit dem Web beschäftigt. Ein Server kann ab 50 Kronen pro Monat **gemietet werden**.

- Serverinstallation (unterscheidet sich bei Windows und Linux)
- Server-Konfiguration
	- Apache-Einstellungen
	- PHP-Einrichtung
	- <a href="/info">Auslesen der aktuellen PHP-Konfiguration</a> (Funktion `phpinfo()`)
	- Ngnix-Routing (verbessert die Serverleistung)

Internet und Webbrowser
--------------------------------

- Web-Browser
- Prinzip von Anfrage und Antwort
	- URL-Adresse
	- Ajax und Ajaj
	- Erzeugung von HTML-Code (Templating-Systeme)

Streicher
-----------------

- Lesen, Schreiben und Verketten von Strings, insbesondere grundlegende String-Arbeit
- Verarbeitung von Zeichenketten
	- Zeichen für Zeichen durchsuchen
	- <a href="/if">Vergleich von Zeichenfolgen</a>
	- String-Ähnlichkeit (Funktionen `levenshtein()`, `similar_text()` und `soundex()`)
	- <a href="/explode">Explode</a>, Aufteilung durch Trennzeichen
	- **Regelmäßige Ausdrücke** zerlegen Zeichenketten nach einer universell konfigurierbaren Maske
	- **Tokenizer** zerlegt Zeichenketten in kleine Teile (Token)
- Serialisierung von Daten in eine Zeichenkette
	- <a href="/json">Json</a>, ein in einer Zeichenkette gespeichertes Javascript-Objekt
