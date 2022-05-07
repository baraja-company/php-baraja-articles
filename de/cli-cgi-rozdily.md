Unterschiede zwischen CLI und CGI
=================================

> id: '79b00e74-b59a-4ae6-8a59-8eecfe2a01ca'
> slug:
> 	cs: cli-cgi-rozdily
> 	de: unterschiede-zwischen-cli-und-cgi
> 
> perex:
> 	- Nejdůležitější rozdíly mezi CLI a CGI. Informace o prostředí.
> 	- Die wichtigsten Unterschiede zwischen CLI und CGI. Informationen über die Umwelt.
> 
> publicationDate: '2021-10-15 10:00:00'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '0964358c913ca647cc947773de87ceda'

PHP kann in verschiedenen Umgebungen laufen. Die gebräuchlichste Umgebung ist `CGI`, die läuft, wenn PHP eine HTTP-Anfrage verarbeitet. Es ist jedoch auch möglich, ein PHP-Skript über das Terminal auszuführen. In diesem Fall handelt es sich um eine so genannte CLI-Aufgabe (Command-line interface).

Die wichtigsten Unterschiede zwischen CLI und CGI
-------------------------------------

- Im Gegensatz zu `CGI SAPI` schreibt `CLI` standardmäßig keine Header in die Ausgabe.
- Es gibt einige `php.ini`-Direktiven, die in `CLI SAPI` außer Kraft gesetzt werden, weil sie in einer Shell-Umgebung bedeutungslos sind:
   - `html_errors`: CLI-Standardwert ist `FALSE`.
   - implicit_flush": Standard-CLI-Wert ist "TRUE".
   - Maximale Ausführungszeit: Der Standardwert der CLI ist 0 (unbegrenzt).
   - register_argc_argv": Standard-CLI-Wert ist "TRUE".
- Das Skript kann Kommandozeilenargumente akzeptieren! Die Variable "$argc" gibt die Anzahl der Argumente an, die an die Anwendung übergeben werden. Und das Feld "$argv" gibt Ihnen ein Array der tatsächlichen Argumente
- Es wurden 3 neue Konstanten für die Shell-Umgebung definiert: `STDIN`, `STDOUT`, `STDERR`. Alle sind Dateihandler für das entsprechende Shell-Gerät. Zum Beispiel ist `STDIN` ein Datei-Handler für `fopen('php://stdin', 'r')`. Sie können also eine Zeile aus `STDIN` wie folgt lesen: `$strLine = trim(fgets(STDIN));`. Das `STDIN` ist bereits für Sie mit Hilfe der `PHP CLI` definiert.
- Das PHP CLI ändert das aktuelle Verzeichnis nicht in das Verzeichnis des ausgeführten Skripts. Das aktuelle Verzeichnis für das Skript wäre das Verzeichnis, in dem Sie den PHP CLI-Befehl ausführen.
- Es gibt eine Reihe von NÜTZLICHEN Optionen für das PHP CLI. Damit können Sie einige wertvolle Informationen über Ihre PHP-Einrichtung und Ihr PHP-Skript erhalten oder es in verschiedenen Modi ausführen.
- In PHP 5 gab es einige Änderungen an den CLI- und CGI-Dateinamen. In PHP 5 wurde die CGI-Version in `php-cgi.exe` umbenannt (früher `php.exe`) und die CLI-Version befindet sich nun im Hauptverzeichnis (früher `cli/php.exe`).
- In PHP 5 wurde auch ein neuer Modus eingeführt: `php-win.exe`. Dies entspricht der CLI-Version, mit dem Unterschied, dass in `php-win` nichts gedruckt wird und somit keine Konsole zur Verfügung steht (es wird keine "Dos-Box" auf dem Bildschirm angezeigt). Dieses Verhalten ist ähnlich wie bei `PHP GTK`.
