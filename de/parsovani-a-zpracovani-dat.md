Parsing und Datenverarbeitung in PHP
====================================

> id: '7dfc991d-a7ee-4e2b-8159-16cef1d27483'
> slug:
> 	cs: parsovani-a-zpracovani-dat
> 	de: parsing-und-datenverarbeitung-in-php
> 
> publicationDate: '2017-10-15 09:54:02'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '33947488a0005f2977e6283c532a08d7'

> Dieser Artikel behandelt Methoden zur Datenverarbeitung in PHP, ist aber noch nicht abgeschlossen.

Also vorläufig nur eine kurze Skizze der Möglichkeiten:

- **Zeichen für Zeichen krabbeln** ist eine sehr alte Methode, die den Code unübersichtlich macht, aber alle anderen Methoden tun dies intern.
- <a href="/explode">Explode</a>, Aufspaltung einer Zeichenkette durch ein Begrenzungszeichen
- <a href="/regex">**Reguläre Ausdrücke**</a> sind der beste Weg, um einfache Zeichenketten zu behandeln
- **Tokenizer**, zerlegt eine komplexe Zeichenkette in Teile (Token) entsprechend regulärer Ausdrücke, z. B. wird PHP so verarbeitet
