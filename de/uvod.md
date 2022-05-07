Einführung in PHP
=================

> id: '66b7fd22-6195-4999-ad8d-b799afdc1876'
> slug:
> 	cs: uvod
> 	de: einfuehrung-in-php
> 
> perex:
> 	- 'Úvodní tutoriál do jazyka PHP, možnosti jazyka, informace o tvorbě webu v PHP.'
> 	- 'Einführendes Tutorial zur Sprache PHP, Sprachoptionen, Informationen zur Webentwicklung in PHP.'
> 
> publicationDate: '2019-09-29 19:25:06'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: db6ba8fe6d5ca28aab62d7302c1d7527

Dies ist ein Online-Kurs zum Erlernen von PHP, der für die allgemeine Bildung konzipiert ist. Sie ist auf dieser Website kostenlos erhältlich. Die Texte dürfen ohne schriftliche Bestätigung nicht in einem kostenpflichtigen Kurs verwendet werden. Sie können die auf der Website verwendeten Beispiele ohne weitere Einschränkungen in Ihrer Anwendung verwenden. [Allgemeine Geschäftsbedingungen] (https://baraja.cz/vseobecne-obchodni-podminky).

Upgrade von HTML
--------------

"Aufrüsten"? Es klingt eher wie eine Medienverarsche, und das ist es auch.

Es gibt kein Upgrade. PHP behält alle Funktionen und Merkmale eines reinen HTML-Dokuments bei, es fügt lediglich neue Schreib- und Bereitstellungsoptionen hinzu. Toll, nicht wahr?

Aus der Entwicklung statischer HTML-Seiten wissen Sie bereits, dass der Code aus Tags besteht, die als in spitze Klammern eingeschlossene Schlüsselwörter definiert sind (z. B. drückt `<b>Hallo!</b>` den fettgedruckten Text **Hallo!** aus).

PHP wird in Form von `<?php` und `?>` Tags in die HTML-Seite eingefügt, innerhalb derer andere Anwendungslogik geschrieben wird. Wichtig ist, dass PHP eine eigene Syntax hat (die Regeln, nach denen der Code geschrieben wird) und, anders als HTML, nicht fehlertolerant ist.

Konkrete Beispiele dafür, wie dies zu tun ist und was die einzelnen Markierungen bedeuten, werden später gegeben. Für den Anfang ist es wichtig, die allgemeinen Prinzipien zu verstehen, wie PHP auf dem Server funktioniert und wie der Code verarbeitet wird.

Der Kommunikationsfluss zwischen dem Benutzer und dem Server
---------------------------------------

Bei einer gewöhnlichen HTML-Seite funktioniert das ungefähr so:

> Der Benutzer sendet eine Anfrage (fragt nach einer bestimmten HTML-Seite), der Server schaut auf der Festplatte nach und sendet genau das zurück, was gespeichert ist. Es passiert nichts Besonderes, und man sollte auch nicht mehr erwarten. Die Seiten werden nur statisch sein, ohne die Möglichkeit einer Serverinteraktion.

Wenn wir jedoch PHP hinzufügen, werden Wunder geschehen:

> Der Benutzer ruft die Seite erneut auf. Der Server öffnet die Datei auf der Festplatte, sieht aber, dass sie nicht nur reines HTML enthält, sondern auch spezielle Tags, die auf ein PHP-Skript hinweisen. Es wertet sie also zuerst aus und sendet dann das, was PHP generiert hat.

Die Auswertung des PHP-Codes erfolgt standardmäßig bei jedem Laden der Seite. In Zukunft werden Sie lernen, wie Sie den Code zwischenspeichern (kompiliert speichern, um ihn schneller verarbeiten zu können).

Der Unterschied zwischen PHP-Skriptverarbeitung und C/C++
------------------------------------------

Vielleicht haben Sie in der Schule gelernt, die Sprache `C` oder `C++` zu verwenden. PHP basiert direkt auf der Syntax der Sprache `C`, und innerhalb des Kernels wird die Sprache `C` verwendet, so dass es gut ist, einige Gemeinsamkeiten und umgekehrt Unterschiede zu kennen.

Das Grundprinzip gängiger kompilierter Programme (die lokal direkt auf Ihrem Computer oder Smartphone ausgeführt werden) besteht darin, die Anwendungslogik in den Arbeitsspeicher zu laden, der direkt mit dem Betriebssystem kommuniziert, das die Eingaben des Benutzers entgegennimmt und dann die Programmausgabe anzeigt. Wichtig ist, dass das Programm vom Start bis zur Beendigung isoliert läuft.

PHP beginnt mit jeder Anfrage zum Rendern einer Seite, lädt jedes Mal den gesamten Code und die Daten neu und beendet sich dann. PHP-Skripte haben daher eine buchstäbliche Yuppie-Lebensdauer und existieren in der Regel nur für einige Millisekunden.

Der Vorteil dieses Ansatzes ist ein höheres Maß an Isolation - wenn etwas schief geht, wird alles beim nächsten Laden der Seite neu geladen. Andererseits ist dieser Ansatz mit höheren Leistungsanforderungen verbunden, da wiederholt eine Verbindung zur Datenbank hergestellt werden muss, Dateien von der Festplatte gelesen werden müssen und so weiter.

In Zukunft werden Sie lernen, dass Sie PHP-Skripte im Arbeitsspeicher geladen halten können, indem Sie die Erweiterung `OP Cache` verwenden, die bei den meisten neuen Servern (ab PHP 7.1) in der Grundkonfiguration eingestellt ist.

Herunterladen von fremden PHP-Skripten
--------------------------

Eine relativ häufig gestellte Frage, die wir mit den Studierenden diskutieren, ist die, wie man fremde PHP-Skripte vom Server herunterlädt und sich deren Quellcode ansieht. Dieser Frage geht die Überlegung voraus, dass der HTML-Code einer Seite leicht in einem Webbrowser angezeigt werden kann.

Die Antwort lautet, dass **PHP-Skripte nicht heruntergeladen werden können**. Das liegt daran, dass der PHP-Code zunächst auf dem Webserver ausgewertet wird und dann der generierte HTML-Code (oder eine andere Ausgabe) an den Browser des Benutzers gesendet wird. Daher kann nur die Ausgabe des PHP-Skripts heruntergeladen werden, nicht das Skript selbst.

Wie funktioniert die Generierung zu HTML?
---------------------------------

Es funktioniert nicht wirklich so, Sie werden auf HTML-Seiten surfen. Das PHP-Skript muss sich immer in einer PHP-Datei befinden.

Bisher waren die meisten von Ihnen daran gewöhnt, riesige Ordner mit Dateien zu erstellen, die auf die Endung **.html** enden. Jetzt werden es viel weniger Dateien sein. Im Extremfall kann es sich um eine einzige Datei handeln.
