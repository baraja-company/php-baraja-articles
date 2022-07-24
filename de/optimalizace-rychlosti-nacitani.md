Wie man die Ladegeschwindigkeit von Seiten effektiv optimiert
=============================================================

> id: '72f1d564-cb70-431c-a3dd-a3fdcaf14292'
> slug:
> 	- null
> 	de: wie-man-die-ladegeschwindigkeit-von-seiten-effektiv-optimiert
> 
> cs: optimalizace-rychlosti-nacitani
> publicationDate: '2021-10-15 15:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '013c8f29c213d3f3f1df3fbe4be8d572'

Wenn ich auf Reisen bin, treffe ich oft auf schlechte Internetverbindungen, was mich als Webdesigner dazu veranlasst, über Designprinzipien nachzudenken, um die Webgeschwindigkeit auch bei einer schlechten Verbindung zu verbessern.

Aus der Praxis habe ich einige nützliche Tricks gelernt:

Wichtige Landing Pages sind oft einfach und es zahlt sich aus, benutzerdefiniertes CSS-Styling zu verwenden.
-----------------------------------------------------------------------------------

Zum Beispiel ist die Anmeldeseite für das CMS oft sehr einfach. Müssen für ein einfaches Formular wirklich das gesamte Bootstrap und andere CSS/JS-Frameworks heruntergeladen werden? Wichtige Seiten sind es wert, dass man sie optimiert und nativen Code schreibt.

Sobald die Seite vollständig gerendert ist, haben wir ein paar Sekunden Zeit, in denen der Benutzer seine Daten eingibt, und wir können die verbleibenden Stile im Browser im Hintergrund herunterladen und zwischenspeichern. Wenn sich der Benutzer anmeldet, hat er Bootstrap wahrscheinlich schon heruntergeladen (und wenn er Edge benutzt, nicht...).

Anstelle von Symbolen können wir oft Emoji verwenden
-----------------------------------

Wirklich! Emoji hat eine Menge praktischer Vorteile:

- Er benötigt nur 4 Bytes und übertrumpft damit jedes Bild.
- Das Herunterladen scheitert nie, so dass der Benutzer immer zumindest ein Symbol und nicht nur eine leere Fläche sieht.
- Anzeige im visuellen Stil des Benutzers
- Derzeit bereits breite Geräteunterstützung
- Spart die Serveranforderung und wir müssen uns nicht darum kümmern, woher wir das Bild bekommen (dies kann über CDN optimiert werden, aber warum, richtig)
- Viele Icons, mit denen Sie wahrscheinlich nichts zu tun haben wollen. Woher bekommen Sie zum Beispiel die Flaggensymbole aller Länder, die Sie in der Verwaltung anzeigen möchten? Emoji verwenden: https://github.com/bara.../country/blob/main/flag-emoji.json

Verwendung kritischer Stile direkt im HTML-Code beim Laden der Website
------------------------------------------------------

Manchmal füge ich die wichtigen CSS-Stile, die das Seitenlayout und das Grundlayout definieren, direkt in das HTML ein. Ich setze ein Limit von 1 KB, das ich versuche, so viel wie möglich hineinzubekommen. Der Nachteil dieses Ansatzes ist, dass die Stile bei jeder Anfrage übertragen werden müssen und nicht zwischengespeichert werden können, andererseits laden sie unvergleichlich schneller als ein Bild.

Ich bin kein Experte für Ladegeschwindigkeiten, [Martin Michalek] (https://www.programia.cz/rozhovor-martin-michalek-rychlost-webu/) kann Ihnen das besser sagen.

Verwenden Sie Ajax!
--------------

Ich verwende immer Ajax, um unwichtige oder langsame Inhalte abzurufen. Es erhöht zwar die technischen Anforderungen der Anwendung ein wenig, aber ich kann es mir leisten.

Ein gutes Beispiel für den Einsatz von Ajax ist fast alles in der Verwaltung. Oft sind sehr viele Daten abzurufen, aber der Nutzer ist nicht an allem interessiert. Wenn der Benutzer nur einen Thin-Javascript-Client heruntergeladen hat und alle Daten über Vue und json geholt werden, wird nur die minimale Menge an Daten jemals heruntergeladen und die Antworten sind augenblicklich.

Wie man das in Vue macht: https://vue.baraja.cz/api-a-ajax-ve-vue-js

Am Backend verwende ich die Bibliothek für Nette: https://github.com/baraja-core/structured-api

Verwenden Sie ein geeignetes CDN
---------------------

Für die Verteilung statischer Inhalte empfehle ich für alle Arten von Websites die Verwendung eines CDN. Wenn Sie nicht wissen, wie man ein CDN einrichtet, sollten Sie zumindest Cloudflare im Proxy-Modus verwenden. Statische Inhalte werden auf der Grundlage der HTTP-Header automatisch im Cache gespeichert. Nicht jeder Hosting-Provider hat Pops gut eingerichtet, und bei langen Strecken können Sie dadurch bei jeder Anfrage Hunderte von Millisekunden sparen.

Geeignetes Bildformat
---------------------

Einer meiner Junioren hat kürzlich ein PNG-Bild auf die Hauptseite der Website gestellt, das ein Foto enthielt und 3 MB beanspruchte. Er sagte, es sei in Ordnung, weil die Seite auf seiner Verbindung schnell geladen wurde (das tut sie ja auch auf seiner Glasfaserverbindung zu Hause, nicht wahr...), und er sagte, dass wir heutzutage eine Menge Daten übertragen, zum Beispiel Videos.

Ich bin in dieser Hinsicht altmodisch und optimiere, was ich kann.

Fotos in JPG, oder besser WEBP. Aber ein Bild im JPG-Format zu speichern, bringt nichts, man muss es durch einen Optimierungsdienst laufen lassen: https://tinyjpg.com

Wenn Sie Bilder in Git haben, kann dieses Tool sie automatisch komprimieren und eine Pull-Anfrage senden: https://imgbot.net. Wenn ein neues Bild hinzugefügt wird, wird die PR erneut gesendet.

Wenn Sie Tausende von Bildern komprimieren müssen (z. B. eine ganze Produktgalerie auf einer Website), verwende ich dazu eine Desktop-Anwendung für Mac: https://imageoptim.com/mac

Durch eine angemessene Komprimierung der Bilder lassen sich in der Regel die meisten Daten einsparen. Manchmal sogar 50 %.
